---
title: Using Devin to Recover from the CrowdStrike Outage
link: https://cognition.com/blog/devin-crowdstrike-outage
source: cognition-com-blog
published: 2024-07-20T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2024-07-20-using-devin-to-recover-from-the-crowdstrike-outage.html
---

The [CrowdStrike incident](https://www.crowdstrike.com/blog/statement-on-falcon-content-update-for-windows-hosts/) yesterday left Windows machines around the world stuck in the infamous Blue Screen of Death. Recovery efforts are ongoing but painful, and sometimes require manually fixing each machine:

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/a5bf059c5709d74da8921ae1c3a64b2cfc3e44d5-701x440.png?w=1600&fit=max)

We wanted to explore how Devin can help.

## Using Devin to Recover from the CrowdStrike Outage

To test Devin’s ability to help, we set up a Windows machine in a cloud environment with simulated CrowdStrike failure conditions. Below, we dive into the fix. You can also [review the Devin session](https://preview.devin.ai/devin/6b3766136b95401c858932250858a5e5?ts=1721453083391) yourself to see the full details.

### Start of the session

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/6d836629ed6e06533d409a6b930d640269bc49ae-1564x1476.webp?w=1600&fit=max)

To start, we instructed Devin to follow a [playbook](https://cognition.notaku.site/product-guides/intro-to-playbooks) to recover machine i-0deda09f7e624a5d8. The playbook was written by a Cognition engineer, based on the remediation steps recommended on CrowdStrike’s blog for cloud Windows machines. It contains 8 steps, including an overview of the task, high level guidance on the approach to take, and details on which specific files need to be removed.

Playbooks are a great way to communicate upfront all the context Devin needs to find the most efficient path to solve the task. With playbooks, there will be fewer clarifying questions or help needed from the user mid-session.

After these initial instructions, Devin completed the rest of the session on its own. Let’s see what it did.

### Devin’s Work Log gives users an overview of what Devin completed

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/fa3b4bbdc78e025ee9fb3ccc77e657a5d476b00d-1364x636.webp?w=1600&fit=max)

Devin’s [work log](https://cognition.notaku.site/product-guides/devin's-work-log) shows that Devin successfully completed all steps of the playbook. One specific step, remove\_crowdstrike\_files, is expanded above to show its execution details — it deleted all files matching Windows/System32/drivers/CrowdStrike/C-00000291-\*.

### Devin’s shell history shows the exact commands that were run

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/5f7a19f1fe7aca364b8d00bf496ebc167cd4eab7-1328x980.webp?w=1600&fit=max)

We see that Devin successfully ran key commands, for example mounting the drive and removing the bad files:

\> ubuntu@ip-10-240-169-238:~$ ssh -i ~/.ssh/devin-us-west-2.pem ubuntu@54.191.93.12 "sudo mkdir /mnt/windows && sudo mount /dev/xvdf1 /mnt/windows"

\> ubuntu@ip-10-240-169-238:~$ ssh -i ~/.ssh/devin-us-west-2.pem ubuntu@54.191.93.12 "sudo rm /mnt/windows/Windows/System32/drivers/CrowdStrike/C-00000291-\*"

### Devin encounters an error it had to debug

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/b1adce4525cda19c11a144f93b869a54852d68b7-1458x1276.webp?w=1600&fit=max)

Here’s another illustrative moment: After stopping the target machine, Devin tried detaching the root volume. The operation failed because the instance had not fully stopped yet. To debug this, Devin ran a command to check the instance state, since it sometimes takes a few seconds to fully shut down, before trying to detach it again. Devin solved this problem without requesting any additional help from the user.

### Run was started with a snapshot

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/e9aa7655c9a26b1f551ac8a68a5af5e9f29d46a1-842x396.webp?w=1600&fit=max)

Here’s a small detail: this session was started with a [snapshot](https://cognition.notaku.site/product-guides/machine-snapshots-guide) called with-us-west-2-pem. The machine snapshots feature allows users of Devin to preload its machine with whatever they want; in this case, private key files used for ssh. Users often use this to preinstall software, pre-authenticate into systems, or have repositories pre-cloned on Devin’s machine. (Snapshot details are only visible from inside the organization that created the session, and won’t show up in the public session link.)

### Devin retrieved Knowledge in the middle of the session.

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/b9da38d99d5c09f179eec4c520696138b081dd56-666x78.webp?w=1600&fit=max)

Devin comes equipped with a Knowledge database that users can add to or edit. In this run, “AWS CLI Best Practices” was automatically loaded to help Devin know the right way to use AWS tools given its CLI-only environment. Not visible in this screenshot alone, but it also includes guidance on how to choose security groups, regions, key pairs, and other configs when launching EC2 instances.

[Knowledge](https://cognition.notaku.site/product-guides/devin-knowledge) is a collection of documentation, tips, custom internal libraries, and other materials that Devin needs to be successful within an organization. Devin will use relevant Knowledge automatically to improve its performance, and automatically suggest Knowledge to add based on what it learns.

### End of the session

![Using Devin to Recover from the CrowdStrike Outage](https://cdn.sanity.io/images/2mc9cv2v/production/3dcb15685037f927a7d3c0c730d537ad7cc2034f-1578x1016.webp?w=1600&fit=max)

At the end, Devin reported successfully completing the task, and confirmed that the machine is now bootable.

[Check out the session yourself](https://preview.devin.ai/devin/6b3766136b95401c858932250858a5e5?ts=1721453083391).

To fix a single impacted Windows machine, it would probably have been faster to do manually instead of creating a playbook and running Devin. But when the same type of task has to be done many times, whether for DevOps, code migrations, or refactors, playbooks with Devin are a powerful feature.
