---
title: Reading and writing are less symmetric than you (probably) think
link: https://fgiesen.wordpress.com/2015/10/25/reading-and-writing-are-less-symmetric-than-you-probably-think/
source: hnrss-org
published: 2026-10-28T09:38:07Z
updated: 2026-10-28T09:38:07Z
first_seen: 2026-11-01T09:08:30.968650873Z
authors:
- luu
content: extracted
html: 2026-10-28-reading-and-writing-are-less-symmetric-than-you-probably.html
remote_preview:
  url: https://s0.wp.com/i/blank.jpg?m=1383295312i
---

I am talking about the I/O operations as used in computing here. A typical example of how this kind of thing is exposed are the POSIX syscalls `read(2)` and `write(2)`, which have the following C function prototypes:

```
ssize_t read(int fd, void *buf, size_t count);
ssize_t write(int fd, const void *buf, size_t count); 
```

Now these are raw system calls; user programs *can* use them directly, but they usually don’t. They normally go through some buffered IO layer; in the C standard library, this means `FILE*` and functions `fread` and `fwrite`, which split `count` into a product of two values in a vestigial nod to record-based IO but are otherwise equivalent. For concreteness, suppose we’re interfacing with actual storage (i.e. not a pipe, socket, virtual filesystem etc.). Then conceptually, a “read”-class operation (like `read` or `fread`) grabs bytes from a file say on a disk somewhere and puts them into the specific memory buffer, and a “write”-class operation takes bytes in a memory buffer and writes them to the disk. Which definitely *sounds* nice and symmetric—but there’s some important behavioral asymmetries between them, especially when errors are in the mix. The reasons have to do with buffering.

### Buffered I/O

In general, file I/O operations in your program will not go directly to a storage device; data instead makes its way through several buffering layers (most of which can be disabled using various flags, but in normal usage these layers are on). These layers are there for good reason: on the kernel side, there’s what’s traditionally called the “buffer cache”. Storage devices are “block devices”, which means they store data in blocks. The block size depends on the device; on old hard disks it used to be 512 bytes, CDs, DVDs etc. tend to use 2k blocks, newer storage devices are now on 4k blocks. Block devices only read entire blocks at a time; that means random byte-aligned IO requests such as “read 100 bytes from disk at byte offset 1234567” or “write 2000 bytes to location 987654” can’t be directly passed to the device at all. The buffer cache is used to translate these requests into block-aligned read and write operations that the device understands; non-block-aligned writes also require reading the previous contents of the block that are not overwritten, and those go in the buffer cache as well. And of course, as the name suggests, it acts as a cache.

On the user-space side, we also have buffers, albeit for a different reason: `read` and `write` are system calls, and as such incur a transition to kernel space and back. They also need to check for and report errors every time they are invoked. And of course they actually need to do the work we want them to do – copy the data from (`read`) or to (`write`) the buffer cache. System call overhead varies between OSes, but it’s safe to assume that the whole process takes at least a couple hundred clock cycles in the best case. So for the overhead not to completely dominate the actual work being done, you generally want to be reading or writing at least a few kilobytes at a time. For scale reference, typical IO buffer sizes as of this writing are 4096 bytes (e.g. Visual C++ 2013 `FILE*`, Go `bufio.Reader`/`bufio.Writer`) or 8192 bytes (e.g. GNU libc `FILE*`, Java `BufferedReader`/`BufferedWriter`).

Often there are more buffers too. For example, most hard drives and RAID controllers have their own caches, and it is not uncommon for user-space code to have several layers of buffering for various reasons. But this is enough to illustrate the basic structure.

All of these buffers are used in much the same way for reading and writing. So where’s the behavioral asymmetry between reading and writing that I’m talking about? You need to think about the state of the world (so to speak) after you call a `read`-type call and how it differs from the state of the world after a `write`-type call.

### What happens when you issue an IO operation

Let’s look at what goes into servicing a `read`-type call first: say you open a C `FILE*` and want to read the first 100 bytes via `fread`. The C standard I/O library notices that its buffers are currently empty, and tries to fill them up, issuing a `read` system call to read say 4k worth of data. The kernel in turn asks the file system where the data for the first 4k of the file is located, checks the buffer cache to see if it already has a copy in memory, and if not, it issues a block read command to the storage device. Either way, the kernel makes sure to get those 4k of data into the buffer cache and from there copies them into the standard IO buffers in user-space memory, then returns. The standard IO library looks at the result of the system call, updates the state of its IO buffers, and then copies the 100 requested bytes into the memory buffer the app supplied.

And what if anything goes wrong? Say the file is smaller than 100 bytes, or there was an error reading from disk, or the file is on a network file system that’s currently inaccessible. Well, if that happens, we catch it too: if something goes wrong filling up the buffer cache, the kernel notices and returns an appropriate error to the I/O library, which can in turn pass errors on to the app. Either way, anything that can go wrong will go wrong *before* the `fread` call returns. All the intervening layers need to do is make sure to keep the error information around so it can be passed to the app at the appropriate time.

Now let’s go the other way round: let’s open a fresh new file with a 4k write buffer [\[1\]](https://fgiesen.wordpress.com/2015/10/25/reading-and-writing-are-less-symmetric-than-you-probably-think/#foot1) and issue a 100-byte `fwrite`. This time, the IO library copies the 100 bytes from the app buffer to the write buffer… and immediately returns, reporting success. The underlying `write` system call will not be executed until either the buffer fills up or is flushed as a result of calling `fflush`, `fseek`, `fclose` or similar.

Quick imaginary show of hands: who reading this habitually checks return codes of `fread` or `fwrite` at all? Of those saying “yes”, who also remembers to check return codes of `fflush`, `fseek` or `fclose`? Probably not a lot. Well, if you don’t, you’re not *actually* checking whether your writes succeeded at all. And while these remarks are C-specific, this general pattern holds for *all* buffered writer implementations. Buffered writing delays making the actual `write` system call; that’s kind of the point. But it implies that error reporting is delayed too!

### More buffers

This type of problem is not restricted to user-space buffering either. The implementation of `write` itself has similar issues: generally, after a successful `write` call, your data made it to the buffer cache, but it hasn’t hit actual storage yet. The kernel will make its best effort to write that data to storage eventually (hopefully within the next few seconds), but if there’s a device error or a system crash, that data could still be lost. Both of these are relatively rare these days, so we don’t worry about them too much, right? Except for those of us who do.

Oh, and while `write` will go to some lengths to make sure there are no nasty surprises when writing to local filesystems (for example, even with delayed write-back, you want to make sure to reserve free space on the disk early [\[2\]](https://fgiesen.wordpress.com/2015/10/25/reading-and-writing-are-less-symmetric-than-you-probably-think/#foot2), lest you run out during write-back), at least on POSIX systems there can still be write errors that you only get notified about on `close`, especially when network filesystems such as NFS or SMB/CIFS are in play (I’m not aware of any such late-reported error conditions on Windows, but that doesn’t mean there aren’t any). Something to be aware of: if you’re using these system calls and are not checking the return code of `close`, you might be missing errors.

Which brings up another point: even on local file systems, you only have the guarantee that the data made it to the buffer cache. It hasn’t necessarily made it to the storage device yet! If you want that (for example, you’ve just finished writing some important data and want to make sure it actually made it all the way), you need to call `fsync` [\[3\]](https://fgiesen.wordpress.com/2015/10/25/reading-and-writing-are-less-symmetric-than-you-probably-think/#foot3) on the file descriptor before you close it. The Windows equivalent is `FlushFileBuffers`.

So, if you make sure to check error codes on every `write`, and you `fsync` before you `close` (again checking errors), that means that once you’ve done all that, you’re safe and the data has successfully made it to permanent storage, right?

Well, two final wrinkles. First, RAID controllers and storage devices themselves have caches too. They’re supposed to have enough capacitors so that if the system suddenly loses power, they still have sufficient power to actually get your data written safely. Hopefully that’s actually true. Good luck. Second, the *data* may have made it to storage, but that doesn’t necessarily mean it’s actually visible, because the metadata necessary to reach it might not have been written yet. Quoting the Linux man page on `fsync(2)`:

> Calling `fsync()` does not necessarily ensure that the entry in the directory containing the file has also reached disk. For that an explicit `fsync()` on a file descriptor for the directory is also needed.

For better or for worse, I can’t recall ever seeing code doing this in the wild, though. I’m honestly not sure what the actual guarantees are that popular Linux file systems provide about these things. If you’re handling really *really* important data, you should probably find out.

### Conclusion and summary

Buffering on the read side is great and pretty much transparent because if anything goes wrong, it will go wrong before you ever get to see the data, and you’ll get a proper error code.

Buffering on the write side is much trickier because it delays actual writing and error reporting in ways that most programmers are *supposed* to be aware of, but usually aren’t. Few are aware of the actual steps necessary to ensure that data made it to storage safely, and some of the OS abstractions involved don’t exactly make things easier (see the `fsync` quote above). Here be dragons.

### Footnotes

\[1\] Full buffering not line buffering mode, in case anyone’s feeling nit-picky.\
 \[2\] Actual block allocation—as in, selecting which physical location on the device file writes will end up—is often delayed in modern file systems, to make it easier to assign mostly-contiguous space to large files where possible. However, even with delayed allocation, you want to keep track of how much space is going to be available on the device once all currently pending writes complete, so that you can return “out of disk space” errors appropriately instead of discovering that you’re out of space 10 seconds after the user exited the app he was using to edit and save his Important Document. Because that would be bad. This sounds as though it’s just a matter of accounting, but it gets tricky with file systems that use extents and not bitmap-based block allocation: getting the last few discontinuous blocks on the device means that you might need extra space to store the file extents! All of which is to say: this stuff is tricky to get right.\
 \[3\] Yes, the name looks like it’s part of the C library buffered IO package, but it’s a proper syscall.
