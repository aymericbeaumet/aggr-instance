---
title: How can you not be romantic about UNIX domain sockets?
link: https://yuvalino.com/how-can-you-not-be-romantic-about-unix-domain-sockets
source: lobste-rs-top-1w
published: 2026-09-14T16:27:12Z
updated: 2026-09-14T16:27:12Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- yuvalino.com via polywolf
labels:
- c
- unix
summary: Comments
content: extracted
html: 2026-09-14-how-can-you-not-be-romantic-about-unix-domain-sockets.html
---

Earlier this summer, I gave a technical iOS talk at the DEFCON 34 convention (search up “[Rage Against the Sandbox](https://yuvalino.com/rage-against-the-sandbox)”) and my demo crashed on stage right at startup. I played it off cool and just re-launched the demo and it all worked pretty nicely.

Honestly, I was so nervous about a 1-day LPE exploit (DarkSword) with 36% success rate I was about to show, that the crash didn’t even bother me. But it was Vegas, so I figured “what the hell” and beat the house with a success on first try on stage (not counting the initial crash as it’s not related to the exploit).

Coming back home, I began finalizing the project before releasing its code publicly and it made me want to dig deeper into the crash issue.

## The investigation

I noticed this crash always occurs only after the iOS device is freshly booted, never on the second time onwards. That’s weird because it means it’s not affected by randomness such as ASLR or multi-thread race conditions.

Let’s get a little familiar with the details. My project is a VM running an SSH server inside an iOS application. The premise is that iOS does not permit applications to create child processes. The VM implements multi-processing semantics by overriding process-creation functions. Instead of creating processes, it creates threads in the same process with duplicated resources and memory. In addition, the VM implements a logical code signing bypass and the demo showed it all coming into play by running an unsigned 1-day exploit over an SSH connection that inherently requires multi-processing for job control and TTY.

Since the TTY spec defines only 1 controlling terminal per process we cannot rely on the iOS kernel’s implementation because we won’t be able to have multiple SSH sessions through the same iOS application process. The VM implements TTY in userspace by creating a pair of UNIX domain sockets connected to each other and performing pre-processing on the data sent into each side (when master sends Ctrl+C, slave gets a SIGINT, etc). The crash happens somewhere along the initialization of the SSH connection, which sets up the master/slave ends of the TTY for that session.

```
// tvm.c
/**
 * for a given VM-managed file-descriptor,
 *   pull out the associated TTY object.
 */
static struct tty *
tty_for_file_locked(struct file *file, int *out_ttymode) {
    // ...

    struct tty *tt = (struct tty *)file->f_data;

    // ...

    struct stat st;
    if (-1 == fstat(file->f_rfd, &st)) {
        // ...
        return NULL;
    }

    if (tt->t_mfd_ino == st.st_ino) {
        *out_ttymode = TTM_MASTER;
        return tt;
    }

    VERIFY(tt->t_sfd_ino == st.st_ino); // sanity only XXX: CRASH HERE!
    *out_ttymode = TTM_SLAVE;
    return tt;
}
```

Since file-descriptors can be `dup()`ed around, I’m keeping the original inode numbers of the UNIX domain socket ends and I distinguish the TTY mode of the file by it (master/slave). The crash is at `VERIFY(tt->t_sfd_ino == st.st_ino)`. That sanity check fails and the VM panics. It seemed to me that there’s some kind of memory corruption out there, because it would make no sense for a file-descriptor to change its associated inode.

The crash itself happens at SSH connection start, when the SSH server modifies some terminal properties of the TTY through its master end, reaching the userspace TTY implementation. The SSH server is dropbear, a popular open-source embedded SSH implementation, and my code hooks operating system functions to divert execution flow into the VM. I assume the problem is in my implementation, and not in any codebase with years of mileage.

After debugging the returned inode numbers from `fstat()` inside the VM, something seemed very weird. It didn’t look like a corruption anymore. It appears like an `fstat()` call on one end of the socket yielded a different inode on the second call to `fstat()` on the same end of that socket (second call is in the code snippet shown above).

## The bug

After debugging a little more trying to convince myself something is wrong in my code (perhaps the socket I `fstat()` changed??), I came to the point where nothing made sense to me. Code seemed good, crash was deterministic on the first run of the demo after reboot, it has to be something else. So, I did the obvious thing I’d been avoiding: Just open the kernel code. And so, I did:

```
// /bsd/kern/uipc_usrreq.c
static int
uipc_sense(struct socket *so, void *ub, int isstat64)
{
    struct unpcb *unp = sotounpcb(so);
    struct socket *so2;
    blksize_t blksize;

    if (unp == 0) {
        return EINVAL;
    }

    blksize = so->so_snd.sb_hiwat;
    if (so->so_type == SOCK_STREAM && unp->unp_conn != 0) {
        so2 = unp->unp_conn->unp_socket;
        blksize += so2->so_rcv.sb_cc;
    }
    if (unp->unp_ino == 0) {
        unp->unp_ino = unp_ino++;
    }

    if (isstat64 != 0) {
        struct stat64  *sb64;

        sb64 = (struct stat64 *)ub;
        sb64->st_blksize = blksize;
        sb64->st_dev = NODEV;
        sb64->st_ino = (ino64_t)unp->unp_ino;
    }

    // ...

    return 0;
}
```

That snippet of `uipc_sense()` implements the logic which pulls out the socket’s inode into the `st_ino` field of `struct stat`. You can see the implementation lazily assigns an inode on the first call to stat on the target socket (`unp->unp_ino == 0`) from some global variable named `unp_ino`.

Can you spot the bug?

If you still want to find it yourself, you better stop reading because the next sentence will reveal the answer. The bug has nothing to do with race conditions over the global variable as evident by the fact it is deterministic but rather the global variable usage itself - it should be `++unp_ino` rather than `unp_ino++`. The `unp_ino` global variable is initialized to zero (as most global data should be), and the check (`unp->unp_ino == 0`) assumes `0` means an uninitialized inode field on the socket. But the first ever call to `uipc_sense()` on the system will cause `unp_ino++` to yield 0 (where `++unp_ino` will yield 1). This causes the first ever socket to have `fstat()` called on it (which receives inode 0), to change its inode on the second stat to something else because the assumption is 0 means an uninitialized inode.

First, while not a very interesting bug from a security standpoint, having found a kernel logic bug on the DEFCON stage, and one that breaks userspace, was surprising for me. At that time, I did not know how long this bug had been in the operating system, but it kept me wondering, how many programs are impacted by this bug? Also, how the hell is my demo the first ever process on the iPhone to call `fstat()` on a UNIX domain socket? Is there some system service on every iPhone out there that might be holding an erroneous inode to a UNIX domain socket?

Second, the fix is super easy on the kernel side. But since my code needs to support versions backwards, I probably want to add an Apple-specific check to re-call `fstat()` if I get 0 as the inode number. This made me curious: If all versions of iOS prior to whichever would fix this bug are impacted, what introduced this bug and how far back does it go?

## The history

The most obvious way to figure out how old the bug is: just pull up the earliest commit of XNU in GitHub and read `uipc_sense()`. This is XNU 123.5 released on March 24, 2001 with Mac OS X 10.0:

```
// /bsd/kern/uipc_usrreq.c
static int
uipc_sense(struct socket *so, struct stat *sb)
{
	struct unpcb *unp = sotounpcb(so);
	struct socket *so2;

	if (unp == 0)
		return EINVAL;
	sb->st_blksize = so->so_snd.sb_hiwat;
	if (so->so_type == SOCK_STREAM && unp->unp_conn != 0) {
		so2 = unp->unp_conn->unp_socket;
		sb->st_blksize += so2->so_rcv.sb_cc;
	}
	sb->st_dev = NODEV;
	if (unp->unp_ino == 0)
		unp->unp_ino = unp_ino++;
	sb->st_ino = unp->unp_ino;
	return (0);
}
```

Simpler code but the bug is still there. This dates 25 years back, to the release of Mac OS X. We start getting a grip on the situation, the bug was there since the first release of Mac OS X shortly after the internet boom. Which means all modern macOS and iOS software were affected by this bug. Nice, so everyone is impacted but I still want to find the origin of the bug.

The XNU kernel released in 2001 is a continuation of the Rhapsody kernel which was based on Mach 2.5 and 4.4BSD kernels. Mach was always the core and the BSD layer built on top of it. Hence, you’d see people refer to XNU as a “hybrid” kernel (2 kernels mashed into 1). But what’s really important is that this kernel is an overhaul of the NeXTSTEP kernel brought into Apple after the acquisition of NeXT in 1997. The NeXTSTEP kernel was first released in 1989 and was based on Mach and 4.3BSD. Since the code was closed-source, it would be much easier to just check the 4.3BSD source code of around that era, 4.3BSD-Tahoe:

```
// /sys/sys/uipc_usrreq.c
uipc_usrreq(so, req, m, nam, rights)
    struct socket *so;
    int req;
    struct mbuf *m, *nam, *rights;
{
    // ...

    switch (req) {
        // ...

    case PRU_SENSE:
        ((struct stat *) m)->st_blksize = so->so_snd.sb_hiwat;
        if (so->so_type == SOCK_STREAM && unp->unp_conn != 0) {
            so2 = unp->unp_conn->unp_socket;
            ((struct stat *) m)->st_blksize += so2->so_rcv.sb_cc;
        }
        ((struct stat *) m)->st_dev = NODEV;
        if (unp->unp_ino == 0)
            unp->unp_ino = unp_ino++;
        ((struct stat *) m)->st_ino = unp->unp_ino;
        return (0);

        // ...
    }
}
```

Instead of taking the form of a function, `uipc_sense()` was then a small case snippet in a large switch-case handling all userspace requests of UNIX domain sockets. The funny thing is you can see the XNU 123.5 and 4.3BSD Tahoe logic remains pretty much the exact same way, so it’s pretty safe to say this bug survived all the way through the NeXT-Apple era. That’s 12 years back from 2001 to 1989, hot damn!

Digging further into some historic “commits” (because there wasn’t any git at that time), it seems that the `unp_ino++` line was last changed at December 20, 1985 (unix-history-repo commit `18a9fea`), where the previous code was:

```
    case PRU_SENSE:
        ((struct stat *) m)->st_blksize = so->so_snd.sb_hiwat;
        if (so->so_type == SOCK_STREAM && unp->unp_conn != 0) {
            so2 = unp->unp_conn->unp_socket;
            ((struct stat *) m)->st_blksize += so2->so_rcv.sb_cc;
        }
        ((struct stat *) m)->st_dev = NODEV;
        ((struct stat *) m)->st_ino = unp_ino++;

        return (0);
```

It seems like before that commit, inodes of UNIX sockets were fundamentally broken: handing out a different inode each `fstat()` call. And the change before that in this area was at May 28, 1985 (commit `628f1f5`):

```
    case PRU_SENSE:
        ((struct stat *) m)->st_blksize = so->so_snd.sb_hiwat;
        if (so->so_type == SOCK_STREAM && unp->unp_conn != 0) {
            so2 = unp->unp_conn->unp_socket;
            ((struct stat *) m)->st_blksize += so2->so_rcv.sb_cc;
        }

        return (0);
```

No `st_dev` or `st_ino` at all, `fstat()` on UNIX domain sockets returned plain zeroes. What captured my attention was that the commit message of this May 28 change (probably extracted from the version control software of that time) was “fake up inode numbers and dev for the naive”. While we can’t know for sure what the original developer meant by that, I think I was just called naive by a 41-year-old commit message.

The story we can infer here was that at around May 1985 the first userspace program relied on `fstat()` for UNIX domain sockets on BSD, and someone cared enough to ask for it to be implemented. Furthermore, that someone cared so much that in December 1985, they asked for the inodes to be consistent for the same socket (as you’d expect) and not just blurt out ever-incrementing numbers for each `fstat()` call. It also seems like that someone was “naive” in the eyes of the kernel developer haha. Well, I can’t know for sure what happened in Berkeley 40 years ago, but it seems like inodes for UNIX domain sockets were really a little feature that may have been prototyped experimentally but wasn’t really re-visited properly. It’s really nice seeing how that little situation moved from the labs of Berkeley over the Bay Bridge to Cupertino and made its mark in the latest versions of the iPhone.

## Takeaways

Verify demos better before going on the DEFCON main stage.
