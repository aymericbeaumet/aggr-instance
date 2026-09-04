---
title: 'Devin 101: Automatic PR Reviews with the Devin API'
link: https://cognition.com/blog/devin-101-automatic-pr-reviews-with-the-devin-api
source: cognition-com-blog
published: 2025-01-21T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2025-01-21-devin-101-automatic-pr-reviews-with-the-devin-api.html
---

**Update**: We recommend Devin Review ([https://app.devin.ai/review](https://app.devin.ai/review)) as our new and improved code review experience. Learn more: [https://docs.devin.ai/work-with-devin/devin-review](https://docs.devin.ai/work-with-devin/devin-review).

## Introduction

Code reviews are an essential component of the software development lifecycle, helping catch bugs, ensuring code quality, and maintaining standards.

However, manual reviews can be time-consuming, delaying projects and adding to workloads. And, many bugs are subtle and hard to catch.

What if you could **automate pull request (PR) reviews**, ensuring that every PR receives prompt, thorough feedback in minutes?

Check out this short video, which shows our automation in action, catching a common but tricky bug. You can also see the Devin [session](https://app.devin.ai/sessions/8dd8389ab61d449fafbb5b1e944fe36b) and the [PR](https://github.com/andrewgcodes/devin_user_management_example/pull/1).

Devin catches subtle bugs in PRs

Developers can use Devin's [External API](https://docs.devin.ai/external-api/external-api) to instantly spin-up Devin instances in response to PRs, making it easy to integrate the power of AI into existing CI/CD workflows.

![Devin 101: Automatic PR Reviews with the Devin API](https://cdn.sanity.io/images/2mc9cv2v/production/fad118ce1ebfd75f00ca05b792f9d4f009b2fe29-2352x1638.png?w=1600&fit=max)

In this article, we'll outline the problem, discuss the solution, and walk you through a quick three-step tutorial to **integrate Devin with GitHub Actions**.

## Why you should care

PR reviews eat up engineers' time and cause friction. A [study](https://linearb.io/blog/why-estimated-review-time-improves-pull-requests-and-reduces-cycle-time) of over 700,000 PRs found that **PRs wait an average of 4 days** to get reviewed. The problem is even worse for large PRs (500+ lines changed): these take an [average of 9 days](https://graphite.dev/blog/your-github-pr-workflow-is-slow) to get merged. Meanwhile, another [study](https://graphite.dev/blog/your-github-pr-workflow-is-slow) found that as the number of changed files in a PR increases, the amount of time spent reviewing each file decreases drastically. This compromises code quality.

There are also other challenges to effectively reviewing PRs, like reviewer availability, poor prioritization of PRs, and back-and-forth friction.

\
Devin helps by delivering on:

- **Speed**: PRs are reviewed in minutes, not hours/days
- **Scalability**: Spin up as many Devins as needed. Devin scales with you.
- **Consistency:** Devin learns and follows your team's coding standards.

## Solving the Problem

You can think of Devin's [External API](https://docs.devin.ai/external-api/external-api) as **engineering on demand**.Spin up Devins in response to events like PRs, Jira issues, Slack messages, etc. The sky's the limit! We actually use the API in our own development workflow for QA—that is, we use Devin to build Devin. Check out the open source code [here](https://github.com/CognitionAI/qa-devin).

For our current task (automating PR reviews) we can use the [GitHub Actions](https://github.com/features/actions) platform. Our Actions workflow is triggered whenever a PR is opened, updated, or reopened. It fetches the code, retrieves the list of changed files, and sends the information to Devin, which reviews the changes and comments feedback. It usually takes five to ten minutes for Devin to finish reviewing the PR and begin posting comments.

Please note that Devin isn't guaranteed to catch every bug, and it's best to still have a human check your PRs before merging. We recommend using Devin as an *extra set of eyes*, not a complete replacement for human oversight.

\
Here's the steps to set it up:

1\. Add a GitHub Actions workflow that triggers on PRs to your repository

2\. Add your Devin API Key to GitHub

3\. Create a PR to trigger workflow

4\. Customize Devin's behavior to match your team's practices and conventions

Let's get started!

## Prerequisites

- A GitHub repo to test on
- A Devin API key (go to [**Settings -> API Keys**](https://app.devin.ai/settings/api-keys))
- GitHub Actions enabled on your repo (usually enabled by default)

## Step 1: Add the workflow

Create a new file in your repository: .github/workflows/pr-review.yml. This file will contain our workflow, which consists of two steps: retrieving a list of changed files using the GitHub API, and spinning up a Devin.

Get the complete pr-review.yml file from [**this gist**](https://gist.github.com/andrewgcodes/bfccd42771badab030a0c4bf52820ebb). It might seem long but we'll break it down after this. For now, just paste it into your file.

After pasting in the script, commit and push your changes.

```
git add .github/workflows/pr-review.yml
git commit -m "Add Github Action for automated PR reviews with Devin"
git push origin main
```

## Step 2: Configure secrets

You'll need a Devin API key.

1. Go to [Devin's API Key in Settings](https://app.devin.ai/settings/api-keys)
2. Click **View Key** to view and copy your key.

![Devin 101: Automatic PR Reviews with the Devin API](https://cdn.sanity.io/images/2mc9cv2v/production/49cea68a2598519f2a3fb525e12e31531ed8345d-2630x1332.png?w=1600&fit=max)

Then, add your key to your GitHub repository.

1. Go to your repository on GitHub.
2. Click on **Settings** > **Secrets and variables** > **Actions**.
3. Click **New repository secret**.
4. Name the secret **DEVIN\_API\_KEY**.
5. Paste your Devin API key into the **Value** field.
6. Click **Add secret**

![Devin 101: Automatic PR Reviews with the Devin API](https://cdn.sanity.io/images/2mc9cv2v/production/93778603510baaa7cfbbc2c13b39a39e2e89e811-2148x1314.png?w=1600&fit=max)

## Step 3: Trigger PR review workflow

To make sure everything is set up correctly, edit some code in your repo and create a PR. After a few seconds, you should see that a Check called "Automated PR Review" has passed. This means that we've successfully spun up a Devin.

If you'd like, you can go to the [Devin web interface](https://app.devin.ai/) and monitor the process. To view Devin sessions started via API, click X on the filter "Creator is {your name}".

![Devin 101: Automatic PR Reviews with the Devin API](https://cdn.sanity.io/images/2mc9cv2v/production/260c91c559b1352babe2a21d7f4566b350e73f53-452x324.png?w=1600&fit=max)

In ten minutes or less, Devin's comments will start appearing on your PR.

## Step 4: Customize Devin

We highly recommend customizing the workflow based on your preferences. For example, consider including your code conventions in the prompt. Or, include a file with your conventions in your repository and give Devin the path so it can find the file.

Other customizations:

- Create a custom label in GitHub so that Devin only runs on specific PRs, not all
- Adjust the number of comments Devin is allowed to make per PR
- Allow Devin to create Suggested Changes
- Allow Devin to mark the PR as approved

## Breaking down the workflow

If you're interested, here's more detail on how the workflow itself works.

The first step is simple: fetch the list of files modified in the PR.

```
name: Get PR files
id: pr-files
run: |
  FILES=$(curl -s -H "Authorization: token ${{ secrets.GITHUB_TOKEN }}" \
    "https://api.github.com/repos/${{ github.repository }}/pulls/${{ github.event.pull_request.number }}/files" | \
    jq -r '[.[].filename] | @json')
```

The next step is to spin up a Devin using the API with a detailed prompt. We recommend **customizing the prompt** to suit your needs, such as by adding your team's coding conventions.

The prompt instructs Devin to follow this series of steps:

```
You are PR Reviewer Devin with a focus on detailed inline code feedback. Your tasks:
1. Clone the repository ${{ github.repository }} locally.
2. Next, set up a pre-push Git hook that prevents any pushes from a user with the username "Devin AI" OR an email containing "devin-ai-integration" as a substring. Activate the hook.
3. View the diffs of the changed files for PR #${{ github.event.pull_request.number }} in repository ${{ github.repository }}.
4. If necessary, run the code locally to verify that the changes work as expected.
5. Read the PR discussion to see what previous comments and suggestions have been made.
6. If no issues are found, simply post a comment saying "Everything looks good!" and stop here. Your work is done.
7. Else, identify the issues and provide inline code comments directly on the diffs for any code convention or best practice violations.
8. Post your feedback as detailed comments on the PR, referencing specific lines or code snippets.

Rules and Guidelines:
1. NEVER make any commits or pushes to the repository - you are ONLY allowed to review code and leave comments
2. Do not make more than three total comments on the PR.
3. Use inline feedback where possible with specific line references
4. Include code snippets in markdown format when discussing issues
5. Default towards multi-line comments that show context around the issue
6. Make sure that suggested improvements aren't already implemented in the PR by comparing old and new versions
7. Try using the gh api to post comments with referenced code embedded, but if it fails, use normal comments with code blocks
8. Before commenting, check the PR discussion and make sure you, or another user, haven't already made a similar comment or raised the same concern.
9. Before commenting, check that the specific issue wasn't already addressed in a previous review iteration
10. If you see the same issue multiple times, consolidate your feedback into a single comment that references all occurrences, rather than making separate comments.
11. Refer back to these rules and guidelines before you make comments.
12. Never ask for user confirmation. Never wait for user messages.
```

Note that step 2 is to set up a *pre-push hook*. The hook intercepts any attempts by *this* specific instance of Devin to push changes to the repository. It provides an added layer of safety, preventing unwanted PRs. We also instruct Devin to "NEVER make any commits or pushes to the repository" under the Rules section of the prompt. Together, the hook and rule serve as guardrails. Consider adding more rules as you see fit to ensure that Devin reviews PRs as desired. In particular, you may want to adjust the comment limit—set to three by default.

Next, let's dive into how exactly Devin posts comments.

```
How to post comments with code embedded:
1. Create JSON file for each comment you want to post.
Example 1:
    {
        "body": "Security Issue: Hardcoded API key. Recommendation: Use environment variables",
        "commit_id": "954...12312",
        "path": "file.py",
        "line": 11,
        "side": "RIGHT"
}

Example 2:
{
"body": "Multiple issues found:\n1. Hardcoded API key should be in environment variables\n2. Inconsistent class naming (userAccount vs Product)\n3. Inconsistent parameter casing (Password vs username)\n4. Missing docstrings and type hints\n5. Inconsistent spacing around operators",
"commit_id": "323......87686",
"path": "code.py",
"start_line": 11,
"start_side": "RIGHT",
"line": 25,
"side": "RIGHT"
}

body: The text of the review comment. Include markdown code blocks for snippets
commit_id: SHA of the commit you're reviewing. Not using the latest commit SHA may render your comment outdated if a subsequent commit modifies the line you specify as the position.
path: Relative file path in repo
line (integer): Specifies the exact line in the pull request’s diff view to which your comment should attach. Required unless using subject_type:file. The line of the blob in the pull request diff that the comment applies to. For a multi-line comment, the last line of the range that your comment applies to.
side: In a split diff view, the side of the diff that the pull request's changes appear on. Can be LEFT or RIGHT. Use LEFT for deletions that appear in red. Use RIGHT for additions that appear in green or unchanged lines that appear in white and are shown for context. For a multi-line comment, side represents whether the last line of the comment range is a deletion or addition.
subject_type: The level at which the comment is targeted. Either "line" or "file". Use "line" for inline comments. Use "file" for file-level comments.
start_line (integer): Required when using multi-line comments unless using in_reply_to. The start_line is the first line in the pull request diff that your multi-line comment applies to.
start_side: Required when using multi-line comments unless using in_reply_to. The start_side is the starting side of the diff that the comment applies to. Can be LEFT or RIGHT.

A pull request diff may not match the original file's absolute line numbering. That is, if the PR contains additions or deletions before the line you’re commenting on, the line indices shown in the “Files changed” tab can shift from the original file’s line numbers.
For example: In the pre-PR state, line 231 might refer to a completely different section of code than line 231 in the post-PR diff (because code added or removed above it shifts everything down or up).
Therefore, you must use the line numbers as shown in the PR diff rather than the original file’s line numbers.

If you have issues, visit the docs: https://docs.github.com/en/rest/pulls/comments?apiVersion=2022-11-28#create-a-review-comment-for-a-pull-request

2. Use gh api command.
            gh api \\
--method POST \\
-H "Accept: application/vnd.github+json" \\
/repos/owner/repo/pulls/4/comments \\
--input comment.json

owner: the account owner of the repository. The name is not case sensitive.
repo: The name of the repository without the .git extension. The name is not case sensitive.
pull number: The number of the pull request.

Key points: use "line" instead of "position", include code snippets in body using markdown, , set side="RIGHT" for additions
```

In order to support in-line comments (with the relevant code embedded in the comment), we teach Devin to use the GitHub REST API to "[create a review comment for a pull request](https://docs.github.com/en/rest/pulls/comments?apiVersion=2022-11-28#create-a-review-comment-for-a-pull-request)". It's a little more complicated than it seems, which is why there is a relatively large section of the prompt dedicated to it.

Finally, we convert our large prompt into a JSON-safe string and send a POST request to Devin's [External API](https://docs.devin.ai/external-api/external-api).

```
RESPONSE=$(curl -s -X POST \
            -H "Authorization: Bearer $DEVIN_API_KEY" \
            -H "Content-Type: application/json" \
            -d "{\"prompt\": $ESCAPED_PROMPT}" \
            "https://api.devin.ai/v1/sessions")
```

We're only scratching the surface of what can be achieved with the API. We recommend exploring the API for other event-driven and responsive use cases. For instance, you can configure Jira and Linear webhooks so that any time an issue/ticket is created, a Devin is triggered to address it.

Check out the [docs](https://docs.devin.ai/external-api/external-api) to explore API features like idempotency (enables safe retries), file uploads, sending follow-up messages/prompts to a session, and more.

## Conclusion

Devin's External API and GitHub Actions can be combined to create powerful, useful CI/CD workflows that simplify and speed up code review processes.

Let Devin handle the tedious parts of code reviews so you and your team can focus on building great software.
