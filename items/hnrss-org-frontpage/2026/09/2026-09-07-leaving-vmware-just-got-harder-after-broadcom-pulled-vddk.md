---
title: Leaving VMware just got harder after Broadcom pulled VDDK downloads
link: https://www.virtualizationhowto.com/2026/09/leaving-vmware-just-got-harder-after-broadcom-pulled-vddk-downloads/
source: hnrss-org-frontpage
published: 2026-09-07T20:32:55Z
updated: 2026-09-07T20:32:55Z
first_seen: 2026-09-08T04:23:50.420238947Z
authors:
- josephcsible
summary: 'Article URL: https://www.virtualizationhowto.com/2026/09/leaving-vmware-just-got-harder-after-broadcom-pulled-vddk-downloads/ Comments URL: https://news.ycombinator.com/item?id=49602699 Points: 133 # Comments: 55'
content: extracted
html: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.html
preview:
  file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.preview-062de995e5ec.webp
  width: 256
  height: 144
  color: '#38414d'
images:
- source: https://www.virtualizationhowto.com/wp-content/uploads/2026/09/vmware-vddk-downloads.png
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-fba74fc41db3.png
    width: 1600
    height: 900
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-6ebe58075862.webp
    width: 48
    height: 27
  color: '#292b34'
- source: https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/vmware-vddk-404-for-the-previous-download-page.png.webp
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-2dd92a6ee3fd.webp
    width: 1170
    height: 660
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-4c89df9a6dbf.webp
    width: 48
    height: 27
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-d47d84722e19.webp
    width: 320
    height: 181
  color: '#f7f8f9'
- source: https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/microsofts-guidance-on-downloading-the-vmware-vddk-from-broadcom.png.webp
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-ee34f0d56f6d.webp
    width: 904
    height: 334
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-8b220713a8ce.webp
    width: 48
    height: 18
  color: '#014173'
- source: https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/redhat-solution-document-citing-changes-to-VMware-VDDK-image-downloads.png.webp
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-b6a3a5ffabe7.webp
    width: 934
    height: 309
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-f9eb18c32649.webp
    width: 48
    height: 16
  color: '#fdfdfd'
- source: https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/nutanix-move-utility-1.png.webp
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-ccc6943c9788.webp
    width: 957
    height: 724
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-c7aa0e8b2538.webp
    width: 48
    height: 36
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-c8e6b07603fe.webp
    width: 320
    height: 242
  color: '#f6f7f8'
- source: https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/platform9-migrate-from-vmware-to-private-cloud-director-with-vjailbreak.jpg.webp
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-6b780ebc9c75.webp
    width: 830
    height: 921
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-2a086501cb2c.webp
    width: 48
    height: 53
  color: '#fcfcfc'
- source: https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/Add-ESXi-host-to-Proxmox-storage-for-Proxmox-import-utility.png.webp
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-804d8fc81c71.webp
    width: 644
    height: 286
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-0550ebaacabb.webp
    width: 48
    height: 21
  color: '#fafafa'
- source: https://www.virtualizationhowto.com/wp-content/uploads/wpforo/avatars/brandon-lee_2.jpg
  original:
    file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-ad2bf9e6ef85.jpg
    width: 150
    height: 150
  variants:
  - file: 2026-09-07-leaving-vmware-just-got-harder-after-broadcom-pulled-vddk.image-6b9bdf5eb77c.webp
    width: 48
    height: 48
  color: '#0049ac'
---

Just when we thought that Broadcom wouldn’t be able to slow down customers moving off of vSphere to another platform. They may just now have done something that will have that net effect. This time it involves VMware’s Virtual Disk Development Kit, which we have always known as the VDDK. [Broadcom has just killed the download](https://www.virtualizationhowto.com/2025/03/broadcom-locking-vmware-updates-to-require-download-token-for-site-id/) of this library, and it will have far reaching consequences if this is a permanent move. Let’s see why this is a cause for alarm and why the VDDK is so important.

## What is the VMware VDDK?

If you have never personally downloaded the VDDK you might assume this is some obscure VMware developer package that most just won’t ever need. And, you are right to some extent if you have no need to use the VDDK directly. However, what you might not realize is just how many tools that you probably ARE using that make use of it. Like what?

Well, here is the kicker. This library is central to many of the migration tools that are used to migrate from VMware to other platforms. It is used by tools like Microsoft Azure Migrate, Red Hat’s Migration Toolkit, Nutanix Move, VMware to KVM migration products, and other tools like [virtv2v](https://github.com/libguestfs/virt-v2v) and **nbdkit**. So, for VMware customers who are trying to figure out what life after Broadcom looks like, this is yet another unexpected roadblock to leaving the platform.

The reason this suddenly matters is that Broadcom has **removed the normal publicly accessible VDDK download pages**, and affected customers are reporting that Broadcom support has told them VDDK is no longer available for general use or download. See the Reddit thread here: [Broadcom VDDK links : r/sysadmin](https://www.reddit.com/r/sysadmin/comments/1vmhxxk/broadcom_vddk_links/).

If you now navigate to the link of the VDDK download where it was previously, you will see the following 404 page:

[![Vmware vddk 404 for the previous download page](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/vmware-vddk-404-for-the-previous-download-page.png.webp)](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/vmware-vddk-404-for-the-previous-download-page.png.webp)

Vmware vddk 404 for the previous download page

Even stranger as it would seem, I have been unable to find a traditional Broadcom announcement explaining this change. So far, there has been no transition notice, deprecation announcement, or replacement tool or library announced.

Instead of this, many migration vendors and customers have seen in the last few day that links they have been pointed to are suddenly returning errors. This is much more interesting than an SDK just disappear. So, the net effect, no matter what Broadcom’s motivation might be is pretty hard for us to ignore at this point.

A VMware technology that is used to move and protect VMware workloads has now become harder for customers and migration vendors to get their hands on. For many customers who may be looking at their exit path from VMware, this feels like just another obstacle placed in the exit path from the platform.

## Evidence of changes to VDDK access

The company ShapeBlue has documented the change as of August 25, 2026. They discovered that the VMware VDDK pages referenced in the VMware to KVM migration documentation were returning error. They then went on to test numerous VDDK URL paths that included version specific paths for VDDK 8 and 9. However, both versions are showing to be unavailable.

ShapeBlue documented the change on August 25, 2026, after discovering that the VMware VDDK pages referenced by its VMware-to-KVM migration workflow were returning errors. ShapeBlue tested numerous VDDK paths, including version-specific locations for VDDK 8 and 9, and reported that all of them were unavailable. You can read ShapeBlue’s detailed announcement on the VDDK removal here: [Broadcom Removes VDDK Pages Without Explanation: What You Need to Know](https://www.shapeblue.com/broadcom-vddk-download-vmware-to-kvm/).

They made the observation that Broadcom has not published a public announcement where they explain the reasoning behind this being removed, withdrawn, or deprecated. Very interesting.

### Broadcom support has seemingly confirmed this was intentional

One of the strongest pieces of evidence we have so far that this is an intentional withdrawal of the bits for the VDDK comes from Broadcom customers who have opened up support cases. You will find the discussion on the Reddit thread interesting. A customer posted the response they received directly from Broadcom Customer Care.

The customer on the Reddit thread reported that Broadcom told them the VDDK was “*no longer available for use or download*“. They also pointed the customer back to backup and recovery products from authorized Broadcom Technology Alliance Partners. Again, read this for yourself on the [Reddit thread here](https://www.reddit.com/r/sysadmin/comments/1vmhxxk/broadcom_vddk_links/).

There is another recent Reddit discussion on the official VMware Reddit thread’s subreddit on the topic. Users there are reporting the same thing. One admin said they opened a ticket with Broadcom and were told by support the VDDK had been intentionally removed and was no longer available through the previous method.

Another discussion in the VMware subreddit contains users reporting essentially the same thing. One [administrator said they opened](https://www.virtualizationhowto.com/2015/07/windows-10-edge-opened-builtin-administrator-account/)\
 a ticket with Broadcom and support confirmed VDDK had been intentionally removed and was no longer available through the previous method. Read the VMware community discussion in this sub Reddit here. The post was **recently removed by VMware moderators** but you can still see the comments on the thread here: [\[ Removed by moderator \] : r/vmware](https://www.reddit.com/r/vmware/comments/1vtqs99/removed_by_moderator/).

So, I think the writing is on the wall here that it is difficult for us to just chock this up to some sort of website glitch where the VDDK has disappeared.

## Microsoft has already changed its documentation on VMware migration

However, we may have even stronger evidence from an equally large company as Broadcom, Microsoft. Microsoft has now updated its guidance on the Azure Migrate process and its agentless migration of VMware workloads over to Azure.

Historically, Microsoft documentation has instructed admins to install VMware VDDK on the Azure Migrate appliance. Now though, Microsoft has added a disclaimer or warning to their official guidance that Broadcom may restrict access to the VDDK.

[![Microsoft's guidance on downloading the vmware vddk from broadcom](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/microsofts-guidance-on-downloading-the-vmware-vddk-from-broadcom.png.webp)](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/microsofts-guidance-on-downloading-the-vmware-vddk-from-broadcom.png.webp)

Microsoft’s guidance on downloading the vmware vddk from broadcom

So, basically, they are saying if you can’t get your hands on the VDDK, then you need to use the agent-based migration instead of the agentless migration method. So, this helps to put the nail in the coffin on the question of whether or not this is a real development or not. You can read the full Microsoft guidance here: [Set up an Azure Migrate appliance for server assessment in a VMware environment](https://learn.microsoft.com/en-us/azure/migrate/how-to-set-up-appliance-vmware?view=migrate-classic).

## Red Hat customers have the same roadblock

Red Hat has also published a support article on August 27, 2026, which you can find here: [Unable to download VMware VDDK images for Migration Toolkit for Virtualization (MTV)](https://access.redhat.com/solutions/7146995). So, this is also an issue for organizations migrating VMware workloads into OpenShift Virtualization. The KB says that customers may receive a “not found” or “access denied” error.

[![Redhat solution document citing changes to vmware vddk image downloads](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/redhat-solution-document-citing-changes-to-VMware-VDDK-image-downloads.png.webp)](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/redhat-solution-document-citing-changes-to-VMware-VDDK-image-downloads.png.webp)

Redhat solution document citing changes to vmware vddk image downloads

Since the software is proprietary to Broadcom, Red Hat is saying they can’t host or redistribute the package itself. So, they are simply asking customers to contact Broadcom support to request access.

Because VDDK is proprietary Broadcom software, Red Hat says it cannot host or redistribute the package itself. Red Hat looks to also be investigating long term alternatives to remove or work around the dependency on the VDDK.

Red Hat Engineering is also investigating longer-term alternatives to remove or work around the dependency. They make mention of using something like a storage copy offload, but this will depend on the underlying storage vendor and its capabilities.

## Nutanix Move impacted

So, yet another environment that is affected by this is Nutanix. Nutanix customers have now hit this snag as well. A Nutanix community user has noted that they were preparing a migration from ESXi to AHV with the Nutanix Move utility but they found that the VDDK link gave them a 404, even after they logged into their Broadcom account.

Read my blog on the Nutanix Move utility here: [Nutanix Move: Migrate from VMware ESXi Free Download](https://www.virtualizationhowto.com/2024/03/nutanix-move-migrate-from-vmware-esxi-free-download/).

[![Nutanix move utility](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/nutanix-move-utility-1.png.webp)](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/nutanix-move-utility-1.png.webp)

Nutanix move utility

The customer in the Nutanix Community post was specifically needing the VDDK 7.0.3.1 and the VDDK 8.0.3.2. You can read the community post here: [vSphere VDDK Download for Nutanix Move](https://next.nutanix.com/ahv-virtualization-27/vsphere-vddk-download-for-nutanix-move-45855). There is another discussion that is similar with a customer deploying Nutanix Move to start moving VMs off their existing VMware 8 environment.

Broadcom support supposedly told them that the VDDK had moved into VMware’s Technology Alliance Program and you had to be a part of that with the right relationship and support access. You can read that particular post here: [VDDK 8.x required for Nutanix Move missing possible issues later](https://next.nutanix.com/move-application-migration-19/vddk-8-x-required-for-nutanix-move-missing-possible-issues-later-45830).

## Platform9 is calling this out directly

Platform9 is one of the vendors that has called this out very vocally and directly. This is because the vJailbreak project is designed to help organizations migrate off the VMware platform. They published an article just a few days ago (September 1st) that explained that Broadcom had restricted public VDDK access and that its traditional VDDK plus NBD migration is now affected when customers don’t already have the files they need for the VDDK.

Check out my blog post on Platform9 here: [Platform9 Community Edition Free Download and Install: Migrate from VMware](https://www.virtualizationhowto.com/2025/02/platform9-community-edition-free-download-and-install-migrate-from-vmware/).

[![Platform9 migrate from vmware to private cloud director with vjailbreak](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/platform9-migrate-from-vmware-to-private-cloud-director-with-vjailbreak.jpg.webp)](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/platform9-migrate-from-vmware-to-private-cloud-director-with-vjailbreak.jpg.webp)

Platform9 migrate from vmware to private cloud director with vjailbreak

Platform9 has other alternatives that uses **storage assisted migration** to move the data onto compatible storage and an “accelerated” option they call it that can use a proxy VM inside VMware to connect more directly to VM storage that doesn’t need the VDDK.

Read the Platform9 post here: [Broadcom Cut Public Access of Virtual Disk Development Kit (VDDK) Overnight](https://platform9.com/blog/vddk-no-longer-available/).

## This “smells” a lot like lock in for VMware customers

This is one of those details where the conversation I think is going to get even more uncomfortable for VMware customers. I think there are some very interesting “developments” in the last few days that may point the needle in a certain direction of thought. But I will let you be the judge of this.

At VMware World just the past few days, supposedly they have brought back [VMware vSphere](https://www.virtualizationhowto.com/e-books/) Standard edition? And, ironically, the VDDK that is used to migrate off the platform has suddenly disappeared. I think so many customers are already having a hard time trying to plan their migrations off (and yes most I know are moving as FAST as they can). This is going to be yet another hurdle depending on the tool and solution you are using to get off of VMware.

I am of the school of thought that even if Broadcom gives VMware Standard away free for a year, it would be hard to convince customers that they need to stick with the platform with all the different changes, oh its here one minute, and gone the next type moves that they have made. Trust is ultimately what is now gone from the platform.

Whatever the case, the migration with third-party tools that rely on the VDDK being available have now gone from:

Before:

- Download the VDDK
- Migrate

To now:

- Contact Broadcom support
- See if they will give you a copy
- If they won’t, see if your storage supports some other migration type
- You may be looking at a different migration process altogether

## VMware to Proxmox migrations are not affected with built-in import utility

Many will ask the question if this will put up a roadblock for VMware to Proxmox migrations. The answer is ultimately, no. Proxmox doesn’t depend on the VDDK to import VMware workloads with the import utility built into the solution. So, you can definitely still use the Proxmox import tool to directly migrate VMDKs over from VMware to Proxmox, [in the home lab](https://www.virtualizationhowto.com/2026/09/7-weekend-projects-that-make-your-home-lab-less-dependent-on-you/) or in production environments.

But, if you are using some type of third-party utility that DOES make use of the VDDK, then yes it would be affected too. Check out my walkthrough on the Proxmox import utility here: [Proxmox New Import Wizard for Migrating VMware ESXi VMs](https://www.virtualizationhowto.com/2024/03/proxmox-new-import-wizard-for-migrating-vmware-esxi-vms/).

[![Add esxi host to proxmox storage for proxmox import utility](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/Add-ESXi-host-to-Proxmox-storage-for-Proxmox-import-utility.png.webp)](https://www.virtualizationhowto.com/wp-content/smush-webp/2026/09/Add-ESXi-host-to-Proxmox-storage-for-Proxmox-import-utility.png.webp)

Add esxi host to proxmox storage for proxmox import utility

So, ultimately, I don’t think Broadcom has blocked the VMware to Proxmox migrations. The truth of the matter is with this additional news, they may have actually sped up the bleed of customers away from the platform as this is just one more reason they have to leave.

See my video training series on Proxmox here: [Proxmox Learning — Premium library](https://www.virtualizationhowto.com/premium/series/proxmox-learning/).

## Wrapping up

Just when we think the VMware by Broadcom saga and drama has died down, there seems to always be a new wrinkle to consider. The removal of the VDDK due to whatever reasons are at play here, has some pretty “interesting” timing, considering they are bringing back Standard edition and trying to slow the bleed of SMB and mid tier companies from the platform. Be sure if you are using a third-party tool, to check and see on the requirements for the VDDK and if you have access to this tool. Let me know in the comments what your thoughts are on this and how your organization is responding to the changes.

### About The Author

![Brandon Lee](https://www.virtualizationhowto.com/wp-content/uploads/wpforo/avatars/brandon-lee_2.jpg)

#### [Brandon Lee](https://www.virtualizationhowto.com/author/brandon-lee/)

Brandon Lee is the Senior Writer, Engineer and owner at Virtualizationhowto.com, and a 7-time VMware vExpert, with over two decades of experience in Information Technology. Having worked for numerous Fortune 500 companies as well as in various industries, He has extensive experience in various IT segments and is a strong advocate for open source technologies. Brandon holds many industry certifications, loves the outdoors and spending time with family. Also, he goes through the effort of testing and troubleshooting issues, so you don't have to.
