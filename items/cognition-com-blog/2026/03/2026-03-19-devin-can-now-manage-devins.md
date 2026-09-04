---
title: Devin can now Manage Devins
link: https://cognition.com/blog/devin-can-now-manage-devins
source: cognition-com-blog
published: 2026-03-19T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-03-19-devin-can-now-manage-devins.html
---

**Devin Can Now Manage a Team of Devins**

Starting today, Devin can break down large tasks and delegate them to a team of managed Devins that work in parallel.

Each managed Devin is a full Devin, running in its own isolated virtual machine with its own terminal, browser, and development environment. Each one can independently run shell commands, execute tests, and verify its own changes before reporting back. Each has its own session link, so you can inspect its work or message it directly.

The main Devin session acts as a coordinator: it scopes the work, assigns each piece to a managed Devin, monitors progress, resolves any conflicts, and compiles the results.

This matters because when one agent tries to handle too many things in a single session, context accumulates, focus degrades, and the quality of each subtask suffers. Each managed Devin gets a clean slate, a narrow focus, its own shell, and its own test runner. And they all run in parallel.

Devin can also read the full trajectories of its managed Devins to understand what worked, what didn't, and where they got stuck, and use that to improve how it breaks down the next task. Over time, each managed Devin makes the next one more effective.

## **What Devin can do**

- **Spin up managed Devins**: Devin breaks a task into scoped pieces and delegates each to a separate session
- **Message child sessions**: send instructions, context, or corrections to any managed Devin mid-task
- **Monitor ACU consumption**: track how much compute each child session is using
- **Put child sessions to sleep or terminate them**: pause or stop any managed Devin that's done or going off track
- **Schedule messages to itself**: set up follow-ups and checkpoints so the coordinator stays on top of progress

## **Try it now**

Here are a few tasks where managed Devins can have an immediate impact. Click any prompt to open it directly in Devin.

### **QA your application in parallel**

[QA all pages in our application for the new light mode. Break this down by page and spin up a managed Devin for each one. Each should navigate to its page, test the components, take screenshots, and report back. Compile a summary of what passed and what failed in an .md file. Use a 2-column (side by side) image layout.](https://app.devin.ai/?prompt=QA+all+pages+in+our+application+for+the+new+light+mode.+Break+this+down+by+page+and+spin+up+a+managed+Devin+for+each+one.+Each+should+navigate+to+its+page%2C+test+the+components%2C+take+screenshots%2C+and+report+back.+Compile+a+summary+of+what+passed+and+what+failed.)

### **Run a large-scale migration**

[Migrate our frontend codebase from one icon library to another. Create a reusable instruction for each independent piece of the migration, then spin up managed Devins to execute each part in parallel. Each should verify its changes by running tests before reporting back.](https://app.devin.ai/?prompt=Migrate+our+frontend+codebase+from+one+icon+library+to+another.+Create+a+reusable+instruction+for+each+independent+piece+of+the+migration%2C+then+spin+up+managed+Devins+to+execute+each+part+in+parallel.+Each+should+verify+its+changes+by+running+tests+before+reporting+back.)

### **Run a security and dependency audit**

[Run a security and dependency audit across our codebase. Spin up one session per service or package (10 at a time) to check for vulnerabilities, outdated deps, and license issues, then compile the results into a single report.](https://app.devin.ai/?prompt=Run+a+security+and+dependency+audit+across+our+codebase.+Spin+up+one+session+per+service+or+package+%2810+at+a+time%29+to+check+for+vulnerabilities%2C+outdated+deps%2C+and+license+issues%2C+then+compile+the+results+into+a+single+report.)

### **Refactor across your codebase**

[Refactor all class components in our codebase to functional components with hooks. Break this into independent batches, spin up a managed Devin for each batch, and compile the results into a set of PRs.](https://app.devin.ai/?prompt=Refactor+all+class+components+in+our+codebase+to+functional+components+with+hooks.+Break+this+into+independent+batches%2C+spin+up+a+managed+Devin+for+each+batch%2C+and+compile+the+results+into+a+set+of+PRs.)

### **Test recently shipped features**

[Find 10 features that were recently touched based on merged PRs, and spin up a session for each one to test it end-to-end. Attach a test report from each session and summarize the overall results.](https://app.devin.ai/?prompt=Find+10+features+that+were+recently+touched+based+on+merged+PRs%2C+and+spin+up+a+session+for+each+one+to+test+it+end-to-end.+Attach+a+test+report+from+each+session+and+summarize+the+overall+results.)

Managed Devins are available now for all users. We look forward to seeing what you build with them.
