---
title: Migrating Your GitHub CI to Hugging Face Jobs
link: https://huggingface.co/blog/github-ci-hf-jobs
source: huggingface-co-blog
published: 2026-06-09T00:00:00Z
updated: 2026-06-09T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-06-09-migrating-your-github-ci-to-hugging-face-jobs.html
preview:
  file: 2026-06-09-migrating-your-github-ci-to-hugging-face-jobs.preview-3fb290dc80bb.webp
  width: 256
  height: 100
  color: '#35322f'
images:
- source: https://huggingface.co/blog/assets/github-ci-hf-jobs/thumbnail.gif
  original:
    file: 2026-06-09-migrating-your-github-ci-to-hugging-face-jobs.image-55b08dc0c0f2.gif
    width: 1064
    height: 416
  color: '#110f0d'
- source: https://github.com/user-attachments/assets/c8b450c3-b801-43dc-97ff-954d9bbaf975
  original:
    file: 2026-06-09-migrating-your-github-ci-to-hugging-face-jobs.image-d34ee8a0417b.png
    width: 1992
    height: 1430
  color: '#0a0e17'
- source: https://github.com/user-attachments/assets/0fc8ac73-f93a-419b-bd80-70da2756f50c
  original:
    file: 2026-06-09-migrating-your-github-ci-to-hugging-face-jobs.image-c198fca30ee6.png
    width: 2634
    height: 926
  color: '#0b0f18'
---

If you have a GitHub repository and you have GitHub Actions enabled, you probably use GitHub-hosted runners for CI. That is the default for many projects because it is simple: add a workflow, write `runs-on: ubuntu-latest`, and GitHub gives you a machine.

That default is convenient, but it also has limits. GitHub Actions can be slow or down for maintenance, the hosted machines are generic, and GPU access is not something most open-source projects can just turn on. For [Trackio](https://github.com/gradio-app/trackio), those limits started to matter. We wanted both reliable CPU CI for basic unit tests and frontend checks, but also GPU CI for tests that need to run on actual CUDA hardware.

So built an alternative: keep GitHub Actions in charge of CI, but run the jobs on [Hugging Face Jobs](https://huggingface.co/docs/hub/en/jobs-overview).

The result: Trackio's CI now runs on Hugging Face Jobs and streams back real-time logs, **cutting our CI time for CPU jobs by about 30% and enabling a whole new test suite that runs on GPU machines**!

In this article, we explain step-by-step how to recreate the same setup for your GitHub repo. If you are using an agent, you can point it to this article, since we provide CLI instructions alongside browser-based instructions for us humans.

Let's start with a quick intro to Hugging Face Jobs!

## [](https://huggingface.co/blog/github-ci-hf-jobs#what-is-hugging-face-jobs) What is Hugging Face Jobs?

[Hugging Face Jobs](https://huggingface.co/docs/hub/en/jobs-overview) lets you run commands or scripts on Hugging Face's serverless infrastructure with almost any hardware flavor. A Job is essentially:

- a command to run
- a Docker image, from Docker Hub or a Hugging Face Space
- a hardware flavor, such as CPU or `t4-small` or `h200` GPU
- optional environment variables and secrets

For example, you can run:

```
hf jobs run python:3.12 python -c "print('Hello world')"
```

or

```
hf jobs uv run --flavor a10g-small "https://raw.githubusercontent.com/huggingface/trl/main/trl/scripts/sft.py" 
```

That makes Jobs a natural fit for CI. CI jobs are already command-driven, already run in clean environments, and often benefit from choosing exactly the right hardware. For ML libraries, the GPU case is especially compelling: you can run a test suite on real GPU hardware without maintaining your own always-on runner.

The key step is connecting GitHub Actions to HF Jobs, which we describe below.

## [](https://huggingface.co/blog/github-ci-hf-jobs#the-architecture) The architecture

For this setup, we created [`huggingface/jobs-actions`](https://github.com/huggingface/jobs-actions), a small bridge that turns a GitHub Actions job into an ephemeral self-hosted runner running inside an HF Job.

The complete flow looks like this:

1. A pull request triggers a GitHub Actions workflow.
2. GitHub queues any job whose `runs-on` label is not available, for example `hf-jobs-cpu-upgrade` or `hf-jobs-t4-small`, and sends a signed `workflow_job.queued` webhook to the dispatcher through the GitHub App.
3. The dispatcher Space verifies the webhook, checks for an `hf-jobs-*` label, mints a short-lived GitHub runner registration token, and starts an HF Job on the matching hardware.
4. The HF Job boots an ephemeral GitHub Actions runner and registers it with the repo using that one-shot token.
5. GitHub assigns the pending workflow job to that runner; the runner executes the CI job, reports status back to GitHub, and exits.

From GitHub's point of view, this is just a self-hosted runner. From Hugging Face's point of view, it is just a Job that launches a container to run the workflow steps from the repo’s GitHub Actions.

## [](https://huggingface.co/blog/github-ci-hf-jobs#step-1-duplicate-the-dispatcher-space) Step 1: Duplicate the dispatcher Space

The first thing you need is the dispatcher. This is a small Docker Space that receives GitHub `workflow_job` webhook events and launches HF Jobs in response.

Create this first because the GitHub App needs a webhook URL, and that URL comes from the Space. This Space should be under your own namespace or under a Hugging Face org that you have write access to.

#### [](https://huggingface.co/blog/github-ci-hf-jobs#web-setup) Web setup

Go to [`huggingface/jobs-actions-dispatcher`](https://huggingface.co/spaces/huggingface/jobs-actions-dispatcher) and click **Duplicate this Space**.

![image](https://github.com/user-attachments/assets/c8b450c3-b801-43dc-97ff-954d9bbaf975)

Use:

```
Owner: your HF user or org
Name: jobs-actions-dispatcher
Hardware: cpu-upgrade
```

Use `cpu-upgrade` for real CI so the dispatcher stays available for GitHub webhooks. `cpu-basic` is fine for testing and will probably work, but it can sleep after inactivity; if GitHub's webhook arrives while it is waking up, the workflow may stay queued forever.

After it builds, open the duplicated Space. You will see a section that says "Required Space secrets," which you can ignore for now. The landing page should display the GitHub App webhook URL you need in the next step. It will look like this:

```
https://YOUR-HF-NAMESPACE-jobs-actions-dispatcher.hf.space/webhook
```

#### [](https://huggingface.co/blog/github-ci-hf-jobs#cli-setup) CLI setup

If you'd prefer to set up the dispatcher Space with an agent or use a CLI workflow:

```
export HF_NAMESPACE=your-hf-user-or-org
export SPACE_ID="$HF_NAMESPACE/jobs-actions-dispatcher"

hf repo duplicate huggingface/jobs-actions-dispatcher "$SPACE_ID" \
  --type space \
  --flavor cpu-upgrade \
  --exist-ok
```

Then set:

```
export DISPATCHER_URL="https://${HF_NAMESPACE}-jobs-actions-dispatcher.hf.space"
```

## [](https://huggingface.co/blog/github-ci-hf-jobs#step-2-create-and-install-the-github-app) Step 2: Create and install the GitHub App

Next, create and install the GitHub App from the dispatcher Space itself. This App needs permission to listen for queued workflow jobs and create ephemeral self-hosted runner registration tokens.

### [](https://huggingface.co/blog/github-ci-hf-jobs#web-setup-1) Web setup

Open your duplicated dispatcher Space:

```
https://YOUR-HF-NAMESPACE-jobs-actions-dispatcher.hf.space
```

In the setup form, enter the GitHub repo whose CI should run on HF Jobs:

```
YOUR-GITHUB-ORG/YOUR-REPO
```

Then click the button to create the GitHub App. GitHub will ask you to choose a name for the App; the name can be anything, as long as it is available in your GitHub account or org. After you submit, the final screen tells you exactly how to upload the App credentials to the dispatcher Space with the `hf` CLI.

**Important note**: you will need to provide an [Hugging Face token](https://huggingface.co/settings/tokens) that has permissions to launch Jobs, corresponding to your personal account or an org under which Jobs should be charged. This token should be saved as the `HF_TOKEN` secret in your dispatcher Space.

Finally, you will install the App on the same GitHub repo you entered in the Space. In the Trackio setup, we installed it on `gradio-app/trackio`.

### [](https://huggingface.co/blog/github-ci-hf-jobs#agent-assisted-setup) Agent-assisted setup

The GitHub App manifest flow is still browser-based, but an agent can follow the same Space-driven path:

```
export HF_NAMESPACE=your-hf-user-or-org
export GITHUB_REPO=YOUR-GITHUB-ORG/YOUR-REPO
open "https://${HF_NAMESPACE}-jobs-actions-dispatcher.hf.space"
```

Paste `$GITHUB_REPO` into the Space, click the GitHub App creation button, choose any available App name, and follow the generated GitHub instructions.

After the App exists, install it on your repo from the App settings page. For a GitHub org, the installation settings are under:

```
https://github.com/organizations/YOUR-GITHUB-ORG/settings/installations
```

## [](https://huggingface.co/blog/github-ci-hf-jobs#step-3-final-dispatcher-settings) Step 3: Final dispatcher settings

At this point, the dispatcher Space should be configured. The GitHub App setup flow generated the commands that upload the App credentials, webhook secret, and Hugging Face token to the Space.

![image](https://github.com/user-attachments/assets/0fc8ac73-f93a-419b-bd80-70da2756f50c)

By default, HF Jobs are launched under the same namespace as the dispatcher Space. Optionally, set `HF_NAMESPACE` as a Space variable if you want to bill jobs to a different Hugging Face user or org:

```
export SPACE_ID=YOUR-HF-NAMESPACE/jobs-actions-dispatcher
hf spaces variables add "$SPACE_ID" -e HF_NAMESPACE=your-billing-namespace
hf spaces restart "$SPACE_ID"
```

The token you set in Step 2 should correspond to this namespace.

## [](https://huggingface.co/blog/github-ci-hf-jobs#step-4-change-runs-on) Step 4: Change `runs-on`

The actual workflow change is small. Instead of:

```
runs-on: ubuntu-latest
```

use one of the labels handled by the dispatcher:

```
runs-on: hf-jobs-cpu-upgrade
```

For GPU tests, use a GPU label:

```
runs-on: hf-jobs-t4-small
```

For any GitHub Action you'd like to run on HF Jobs, this 1-line change is all you need!

## [](https://huggingface.co/blog/github-ci-hf-jobs#step-5-test-it-out) Step 5: Test it out

To add a minimal smoke-test workflow from the CLI:

```
mkdir -p .github/workflows
cat > .github/workflows/hf-jobs-test.yml <<'EOF'
name: HF Jobs Test

on:
  pull_request:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  test:
    runs-on: hf-jobs-cpu-upgrade
    steps:
      - uses: actions/checkout@v4
      - run: echo "Hello from Hugging Face Jobs"
EOF

git add .github/workflows/hf-jobs-test.yml
git commit -m "Run CI on Hugging Face Jobs"
git push
```

To verify from the CLI:

```
gh run list --repo YOUR-GITHUB-ORG/YOUR-REPO --limit 5
hf jobs ps --namespace "$HF_NAMESPACE"
hf spaces logs "$SPACE_ID"
```

You should be able to see logs just like a regular GitHub Action—for example, in this [Trackio PR #565](https://github.com/gradio-app/trackio/pull/565).

And that's it!

*Note on choosing the right Docker image*

Our first CPU setup used `ubuntu:22.04` and installed missing system packages during every run. That worked, but it was slower than it needed to be. GitHub's `ubuntu-latest` image includes a lot of developer tooling by default; a bare Ubuntu image does not.

For Trackio, the UI tests need Playwright browsers, Node, ffmpeg, sqlite, git, and normal Linux build dependencies. Hugging Face Jobs supports using any [Docker image](https://huggingface.co/docs/hub/jobs-popular-images), so we switched to the Microsoft Playwright image, which worked well:

```
mcr.microsoft.com/playwright:v1.60.0-jammy
```

For GPU jobs, we used:

```
nvidia/cuda:12.4.0-runtime-ubuntu22.04
```

## [](https://huggingface.co/blog/github-ci-hf-jobs#results) Results

Here are the numbers from the Trackio CI:

| Runner setup                    | Runtime | Compared to GitHub average    |
| ------------------------------- | ------- | ----------------------------- |
| GitHub `ubuntu-latest` baseline | `1m40s` | baseline                      |
| HF Jobs CPU, Playwright image   | `1m10s` | `-30s`, about `30%` faster    |
| HF Jobs GPU, `t4-small` label   | `45s`   | no GitHub-hosted GPU baseline |

The biggest win was GPU CI. The Trackio GPU check ran on HF Jobs and passed in `45s`, costing less than a cent at the `t4-small` rate for that duration.

The CPU result was also encouraging. With the right image, the Linux test job was faster than the GitHub-hosted baseline. That suggests HF Jobs can be a practical CI backend, especially for ML projects that need custom images or accelerators.

Logs were another pleasant surprise. GitHub Actions logs are useful, but the web UI can be heavy for large logs. HF Jobs logs are easy to fetch from the CLI:

```
hf jobs logs <job_id> > logs.txt
```

That makes them easy to inspect with local tools or coding agents. In our bridge, we also mirrored the GitHub Actions job log into the HF Job log, so either system had enough information to debug a run.

Finally, although we didn't need them for Trackio's CI, HF Jobs also [supports mounting volumes](https://huggingface.co/docs/huggingface_hub/en/guides/jobs#mount-a-volume), which can be very helpful if you need to load datasets or models from Hugging Face quickly as part of your CI.

Hopefully, this gives you all you need to try HF Jobs for running your GitHub Actions!
