---
title: Devin December '24 Product Update
link: https://cognition.com/blog/dec-24-product-update
source: cognition-com-blog
published: 2024-12-03T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2024-12-03-devin-december-24-product-update.html
---

By The Cognition Team12.03.24

## Collaborating with Devin through Slack now better mirrors the experience of working with your human teammates

As bug reports and frontend edge cases are reported in Slack, try tagging @Devin in thread:

- For complex tasks, Devin offers to wait for your confirmation -- configure this behavior in [](https://preview.devin.ai/behaviors)[Library > Behaviors](https://preview.devin.ai/behaviors)
- Devin proposes new [Knowledge](https://docs.devin.ai/Onboard_Devin/knowledge) it should remember for future conversations in thread
- Try using Devin’s sessions list as your todo list — after tagging Devin in Slack, Devin works while you’re busy and updates Slack when the PR is ready. Archive Devin sessions once the task is done

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/ceea3a78b29376983bf76c284559a7b7b39a0765-852x480.gif?w=1600&fit=max)

## Improved collaboration on PRs

Ask Devin to create a PR! Recent improvements:

- When PRs receive comments or fail lint, Devin automatically wakes up to address
- Click “PR Preview” under the session title to check the changes Devin’s made before a PR is created
- Devin’s PRs clearly link to Devin sessions and the user requesting the PR

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/db08fa33c20684ff373af82e62075f686eaeba70-3108x1892.png?w=1600&fit=max)

## Talk to Devin from your IDE (Beta Access)

Hand off async work to Devin while you focus on your primary task. Review when convenient.

- Works in conjunction with Copilot and Cursor
- Devin’s just a shortcut away (Cmd+G)
- Keep track of your active Devins
- Review and accept code directly in your local IDE

[Install the Devin Extension](https://marketplace.visualstudio.com/items?itemName=cognition.devin) to get started.

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/6aba68730a4747e6268b2979c937cbb820d1d3f4-580x326.gif?w=1600&fit=max)

## For repetitive engineering tasks, try the Devin API with structured input/output

Programmatically create Devin sessions and retrieve results (including structured output) using our new REST API:

- Integrate Devin into your own applications
- Write scripts to kick off multiple sessions in parallel
- Build powerful automation workflows on top of Devin

Specify a structured output format in your prompt, for example:

Devin, we're using auth0 instead of clerk - can you remove clerk support from the provided file? Output format: {file\_path: string, lines\_edited: int, success: bool}

View structured output on any session page with CMD+I, or click “Show structured IO” in the dropdown menu to the right of your session title.

Read our [external API docs](https://docs.devin.ai/external-api/external-api) to learn more.

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/e2003c8695e4ece669a01ab5962e94a46c9c326a-2518x1246.png?w=1600&fit=max)

## Repo Knowledge

Devin will now automatically scan your repositories and generate [Repo Knowledge](https://docs.devin.ai/Onboard_Devin/knowledge). This allows Devin to more quickly and successfully do real work for you in your existing codebases. You can always add and edit your own Knowledge manually in [Settings > Knowledge](https://preview.devin.ai/knowledge).

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/63ebab1cb6affef338b3ad1eeea91c32d2e1ba78-615x116.png?w=1600&fit=max)

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/fa59ac640025b46cb41ddd0e7f8dde6668607f13-1305x949.png?w=1600&fit=max)

## Configure Devin’s behaviors, including Agency

For certain tasks, much of the work needed is figuring out what should be done and aligning on the approach. Devin will now automatically detect more complex tasks and spend time investigating the repo and proposing a plan before beginning execution.

When Agency is turned off, Devin will wait for you to approve its plan before proceeding. [Configure Behavior such as Agency in Settings](https://preview.devin.ai/behaviors).

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/109ee1ec0dd5879a5e38704010fb5c7d4c0d511e-2874x752.png?w=1600&fit=max)

## Onboard Devin + configure Devin’s Workspace

Devin requires onboarding in order to be successful, just like any new human engineer on their first day.

Devin now guides you through the onboarding it needs, including setting up Devin’s Workspace. **Setting up Devin’s Workspace significantly improves Devin’s performance on your codebase.** Imagine if every time you started a task, your laptop and part of your memory were wiped - that’s what happens to Devin without setup!

Behind the scenes, Devin’s Workspace resets to a saved machine state at the start of every session. By default this machine state includes all the repositories you’ve added and set up at [preview.devin.ai/workspace](http://preview.devin.ai/workspace). [Learn more in our docs](http://localhost:3001/Onboard_Devin/snapshots).

![Devin December '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/9a9e98374a93cabf7dd70f677512053a7258e8cc-2106x1524.png?w=1600&fit=max)

## Devin is now faster!

Speed has improved at every level — from time to first action, to the speed of navigating our web application. Time to create a [](https://docs.devin.ai/Onboard_Devin/snapshots#advanced-machine-snapshots)[Snapshot of Devin's machine](https://docs.devin.ai/Onboard_Devin/snapshots#advanced-machine-snapshots) has been reduced from ~30 minutes to ~15 seconds and time to first message has been reduced from ~25 seconds to ~10 seconds.

## Devin for Enterprises

Enterprise users now have more options to configure Devin to meet your organization’s needs, including:

- **Single sign-on with Okta**
- **Auto-Join for Company Domains:** Allow any user with a company email to join Devin without individual invites
- **Customized Onboarding:** Tailor example sessions and suggested prompts to guide your organization’s users to Devin’s most valuable use cases
- **Usage Insights:** Automated email alerts to track your usage over time

## Hire Devin

We are working hard to scale up access to Devin, and we continue to do weekly invite releases. To start using Devin for engineering work, [join the waitlist](https://www.cognition.ai/get-started) or get in touch at [info@cognition.ai](mailto:info@cognition.ai).
