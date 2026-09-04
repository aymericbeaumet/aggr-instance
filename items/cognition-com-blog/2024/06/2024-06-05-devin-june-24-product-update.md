---
title: Devin June '24 Product Update
link: https://cognition.com/blog/june-24-product-update
source: cognition-com-blog
published: 2024-06-05T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2024-06-05-devin-june-24-product-update.html
---

## Use Devin’s Browser, Editor and Shell

It can sometimes be more convenient to directly take actions for Devin, rather than providing instructions for Devin to follow.

We’re excited to share that you can now directly use Devin’s machine. The new “Use Devin’s Machine” button in the web interface opens VSCode in a new tab. Using VSCode, you can directly read and edit Devin’s files, as well as open up a terminal in Devin’s machine.

Using VSCode, you can directly read and edit Devin’s files

Similarly with the browser, the new Interactive Browser lets you directly use Devin’s browser. This is especially helpful for browser tasks where Devin may require assistance, such as completing CAPTCHAs.

Interactive Browser lets you directly use Devin’s browser

## With Playbooks, Devin is better equipped for repetitive, multi-step engineering tasks

We’ve worked with a number of enterprises using Devin for tasks that are recurring in nature, such as adding unit tests or performing code migrations.

Playbooks are documents that provide Devin with the steps and context needed to complete common engineering tasks. They’re like programs, without the rigid syntax. With Playbooks, you can quickly specify not only the steps Devin needs to take to successfully complete a task, but also the success criteria and guardrails.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/4859cb8678c388f46cec86b5ec97c806d367f06a-1005x721.png?w=1600&fit=max)

Playbooks can be easily shared within an organization, so once one engineer successfully coaches Devin to complete a task, other engineers can replicate that success with similar tasks.

We also launched a community gallery of Playbooks for tasks that are common across our users - e.g. full stack app development and deployment, web scraping, and Python notebook to script conversion. Playbooks minimize the back and forth needed to guide Devin to complete a task successfully, as they communicate the necessary instructions and details upfront.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/e1d19dc9170468baf3f11414d6dd74d515e8b95d-1639x950.png?w=1600&fit=max)

## Quickly equip Devin with necessary organizational context

Just as with onboarding a new engineer, onboarding Devin requires an initial investment in knowledge transfer.

With the new Knowledge feature, you can share documentation, tips, custom internal libraries, and other materials that Devin needs to be successful within your organization. Devin will use relevant Knowledge automatically to improve its performance in your environment.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/f0717e6baff61cd11bc87fb20650b4e49f2af0d2-1219x690.png?w=1600&fit=max)

We recommend using Knowledge in combination with Playbooks.Create Playbooks for common, recurring tasks, and make sure to include best practices for those tasks in the Playbook. Add Knowledge to teach Devin general context about your organization that is relevant for all runs, with and without Playbooks.

## Persisting Devin’s dev environment

With Machine Snapshots, Devin is able to hit the ground running in every new session.

Machine Snapshots are ‘save’ states for Devin. After you take a snapshot, you can start from that machine state (with everything you’ve downloaded/installed) on any future Devin run. You can use Machine Snapshots to:

- Perform one-time installation and setup and start new sessions from that point, every time
- Resume from a previously ended session

For each snapshot, you can also set a list of startup commands that will be run at the beginning of every run. The commands will run in sequence. For example, with startup commands, you can have Devin cd your-repo, git pull and npm install at the beginning of every session.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/eaf77feb5dbafff0bd2c1b2672fbeb0fa6c5a7d9-1339x930.png?w=1600&fit=max)

## Work with Devin via Slack, Github, or Linear

There are now new ways of working together with Devin.

First, Devin can now be triggered in an event-driven way. For example, Devin can get to work immediately when build fails, or when it’s assigned a Linear ticket.

Devin will also respond to Slack messages, and will keep you updated on progress via Slack threads. You can also turn on Slack or desktop notifications for sessions started via the web app.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/f1ca7d578f24efc401fc2b89249d58f3ea630c35-1986x1342.png?w=1600&fit=max)

With native GitHub integration, Devin can also quickly create or review pull requests and respond to comments on its PRs.

## Secure secrets sharing and authentication

The new Secrets manager provides flexibility and security in managing personal and organization secrets, such as API keys, passwords, and authentication tokens. Define organization-level secrets with Devin once, and Devin will automatically retrieve relevant secrets as needed in future sessions. Secrets can alternatively be provided for a single session. Secrets are stored securely and revocable at any time.

You can also choose to share cookies with Devin, which enables Devin to login to websites without needing a password.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/2ab65f30be9f063e9a3806916f7ec3741c8ebf45-2398x934.png?w=1600&fit=max)

## New controls for session state

Directly pause, terminate, or “keep alive” a session with Devin in the web interface. With “Keep alive”, sessions stay alive until users manually terminate them. This allows users to continue accessing any services the session may be running.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/785dbbf824365c2e1b1755ddb5a4e69d44f47ce5-1991x310.jpg?w=1600&fit=max)

## Improved visibility into Devin’s actions

It’s now faster and easier to verify Devin’s work. With Global Work Diff, users can pull up a summary of all code changes Devin has made up to any point in time. For more detail, users can also view the file edit histories for any individual file.

Users can pull up a summary of all code changes Devin has made up to any point in time

Finally, with Command History, users can see a summary of all commands Devin has run across all shells.

![Devin June '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/35b2f4a70b3bf39dc6e5ae1b092fee5aff9f539a-2000x2505.png?w=1600&fit=max)

## Hire Devin

We are working hard to scale up access to Devin, and we continue to do weekly invite releases. To start using Devin for engineering work, [join the waitlist](https://cognition.com/get-started) or get in touch at [info@cognition.ai](mailto:info@cognition.ai).
