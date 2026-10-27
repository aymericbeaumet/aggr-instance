---
title: Coding From the Himalayas With an Offline AI Agent
link: https://latent.space/p/offline-coding/
source: latent-space
published: 2026-10-27T14:41:25Z
updated: 2026-10-27T14:41:25Z
first_seen: 2026-10-27T16:00:47.207930047Z
authors:
- Rittika Jindal
summary: How to set up and run an offline coding agent using OpenCode, Ollama and Qwen.
content: extracted
html: 2026-10-27-coding-from-the-himalayas-with-an-offline-ai-agent.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/425b6203-2bf9-4c0a-9545-27f2704fa9aa/himilayas-no-wifi-1280x720.png?t=1793103846
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/425b6203-2bf9-4c0a-9545-27f2704fa9aa/himilayas-no-wifi-1280x720.png?t=1793103846
---

How to set up and run an offline coding agent using OpenCode, Ollama and Qwen.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/b8261806-4860-420e-8277-2c8411dece16/himilayas-no-wifi-1280x720.png?t=1793112695)

When we talk about **working remotely**, we take a lot of things for granted, especially reliable internet and power. But sometimes, working remotely means trying to get something done on a flight, during a long train ride, or somewhere with little or no network.

I moved to the foothills of the Himalayas a year ago. I love the peace of living here, but it also comes with its own challenges, **including network and power outages**. Things I didn’t think about as much before are now part of planning my work.

I work as a **Principal Engineer at Thomson Reuters**, where I spend most of my time coding and reviewing my teammates’ code. Coding agents are already a big part of my everyday workflow, which made me curious about **how much of that workflow I could keep using offline.**

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/e8e953e3-8a45-405c-b931-b45ba1f9a77c/image.png?t=1793105992)

Working from the foothills of the Himalayas. Photo of my workspace.

If you’re someone like me, who enjoys working with technology but also finds peace in quiet, remote places where the network doesn’t always cooperate, this article is for you. It’s also for anyone who has wondered **whether a coding agent can run entirely on their laptop, with the internet switched off.** I tried running a coding agent locally on my laptop to see how much work I could actually get done offline.

## **What can we do offline?**

A project already on your laptop gives us plenty to work with. We can ask an agent to explain an unfamiliar function, investigate a failing test, refactor a small piece of code, or help finish a feature. These tasks can run offline **when the model, project dependencies and test tools are available locally.** Downloading a new package or looking something up on the web will still need a connection.

## **Where OpenCode fits**

[**OpenCode**](https://docs.ollama.com/integrations/opencode?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is an **open-source coding agent** that works with your project. You give it a task, and it can read files, propose edits and run commands such as the test suite. In our setup, its permission prompts let us review commands and changes before allowing them.

I picked OpenCode because it lets me keep that workflow in the terminal while connecting to a model running on my own laptop. **Ollama** handles running the model; OpenCode handles the interaction with the project.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/a4aa1b11-0361-4d02-b625-b4fcc4575b5b/image.png?t=1793103654)

OpenCode on my Mac, with Qwen3.5 9B selected as the local model.

Open-weight models, including [**Qwen**](https://huggingface.co/Qwen/Qwen3.5-9B?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), make this possible: their weights, the learned parameters used to generate responses, can be downloaded and run locally. **With Ollama running Qwen on the same laptop, OpenCode can send its requests to a local service.** We download everything first, then disconnect the internet for the coding task.

[**Aider**](https://aider.chat/docs/llms/ollama.html?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is another option if you prefer a guided editing workflow; it also supports Ollama. We are using OpenCode throughout this walkthrough so that the setup and results stay consistent.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/548a3ad7-ac2b-4943-8d2d-f93a8bdfb172/image.png?t=1793103653)

OpenCode works with the project files and sends model requests to Ollama on the same laptop. Ollama runs Qwen locally.

## **Choosing an open-source model**

Qwen comes in different sizes. I wanted a model we could run locally that could also use an agent’s tools. For context, here are published results for two Qwen sizes and another open-weight option, as reported in [Qwen’s model card](https://huggingface.co/Qwen/Qwen3.5-9B?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv).

[LiveCodeBench v6](https://livecodebench.github.io/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) tests code generation on programming problems. [BFCL-V4](https://gorilla.cs.berkeley.edu/leaderboard.html?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) evaluates function calling, which matters when a model needs to choose a tool and give it the right arguments. Higher scores are better on both.

| **Model**                  | **LiveCodeBench v6** | **BFCL-V4**  |
| -------------------------- | -------------------- | ------------ |
| Qwen3.5 4B                 | 55.8                 | 50.3         |
| **Qwen3.5 9B (used here)** | **65.6**             | **66.1**     |
| gpt-oss-20b                | 74.6                 | Not reported |

The above table scores reported in [Qwen’s model card](https://huggingface.co/Qwen/Qwen3.5-9B?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), not measurements from this laptop. “Not reported” means the source does not provide that score.

**We’re using Qwen3.5 9B for this walkthrough.** Its stronger published coding and tool-calling results than the 4B variant made it a useful starting point. The table doesn’t measure laptop speed or the whole OpenCode workflow; our offline file edits and tests are the check for that.

### **Model size and memory**

|                            |                      |
| -------------------------- | -------------------- |
| **Model**                  | **Approx. download** |
| Qwen3.5 4B                 | 3.3–4.0 GB           |
| **Qwen3.5 9B (used here)** | **6.6–7.6 GB**       |
| Qwen3.5 27B                | 17–20 GB             |

Download size isn’t a RAM requirement. [Voltti budgets 12–16 GB of memory](https://voltti.ai/en/insights/running-ai-locally-what-computer-do-you-really-need?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) for a 6.6 GB Qwen3.5 9B build. Our 64k-token OpenCode configuration may need more, so leave room for context and other apps.

## **How OpenCode performed in benchmarks**

OpenCode has also been tested beyond small demos. The research preprint [The Scaffold Effect in Coding Agents](https://arxiv.org/html/2607.22585v1?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) compared OpenCode, Goose and OpenHands-SDK on 50 Terminal-Bench Pro tasks using two cloud models. Here are the pass rates reported in Table 2.

|                  |                    |                    |
| ---------------- | ------------------ | ------------------ |
| **Coding agent** | **Qwen3.6 Plus**   | **MiniMax M2.5**   |
| Goose            | 48% \[34–62%\]     | 38% \[24–52%\]     |
| **OpenCode**     | **50% \[36–64%\]** | **46% \[32–60%\]** |
| OpenHands-SDK    | 50% \[36–64%\]     | 46% \[32–60%\]     |

The brackets show 95% confidence intervals. The authors caution that most differences in pass rate were not statistically clear.

The study also reported **substantially higher token use for OpenCode**: about 40 times Goose’s tokens per solved task. These runs used cloud APIs and different models from ours, so they do not predict how local Qwen3.5 9B will perform.

## **Initial setup**

While you still have access to the internet, follow the complete setup in the [GitHub README](https://github.com/RittikaJ/opencode-qwen-local-setup?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). It covers installing the tools, downloading the project, and installing [OpenCode 1.19.4](https://github.com/anomalyco/opencode/releases/tag/v1.19.4?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). The project includes the examples, tests and configuration for using Qwen locally. Once that is ready, keep Terminal A open in the project folder.

Before starting, it helps to know **what opencode.json does**. This is [OpenCode’s project configuration](https://opencode.ai/docs/config/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). In our file, model and small\_model both select Qwen3.5 9B, and baseURL points to Ollama at http://127.0.0.1:11434/v1. Only the local provider is enabled. It also asks before editing files or running shell commands, and blocks the web tools. The [full configuration](https://github.com/RittikaJ/opencode-qwen-local-setup/blob/main/opencode.json?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is in the repository. We still disconnect the network for the offline test.

We’ll use Terminal A for OpenCode and our tests. In Terminal B, start Ollama and leave it running. This command keeps the model service on [localhost](https://localhost/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), an address inside the same Mac, and [disables Ollama’s cloud features](https://docs.ollama.com/faq?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv).

```
OLLAMA_HOST=127.0.0.1:11434 OLLAMA_NO_CLOUD=1 OLLAMA_CONTEXT_LENGTH=65536 ollama serve
```

Back in Terminal A, download Qwen and wait for the download to finish. We’ll check that the agent responds before disconnecting the internet.

```
ollama pull qwen3.5:9b
```

## **Starting with an expense calculator**

The example adds five expenses, including a refund. The correct total is 25.50. Run the starting program in Terminal A.

```
python3 expense_total.py
```

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/d882978d-54a4-4c7f-92a4-413709fac2d1/image.png?t=1793103653)

The starting program prints 8.00, the value of the last expense.

The function replaces its total on every iteration. Before asking the agent to fix it, run the existing tests.

```
python3 -m unittest -v
```

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/3993d477-4a99-4500-bad1-fae63bd5bbe3/image.png?t=1793103653)

Three cases fail: decimal arithmetic, refunds and the sample expenses. Empty and single-item input already pass.

Using existing tests gives the agent a concrete target. It also gives us a way to check the result independently of its explanation.

## **Trying OpenCode while online**

While online, open OpenCode directly from the project folder. The [--pure](https://opencode.ai/docs/cli/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) option disables external plugins. The [project configuration](https://opencode.ai/docs/config/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) points both the main and helper model at Ollama; disconnecting the network later is what makes the run offline.

```
.tools/node_modules/.bin/opencode --pure
```

Check that Qwen3.5 9B local is selected. Enter this diagnostic prompt in OpenCode.

Read expense\_[total.py](https://total.py/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) and run the tests. Explain the bug without editing any files.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/143f375c-d3f5-467a-a7b0-7bb554d2ec16/image.png?t=1793103653)

OpenCode receives the diagnostic prompt with Qwen3.5 9B local selected.

OpenCode read the source and searched for the tests. It initially proposed pytest, although this project uses Python’s built-in unittest. The permission prompt gave me a chance to inspect the command before allowing it. For the next task, I would include the exact test command in the prompt: python3 -m unittest -v.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/2567cb0b-a166-46f3-9ffe-5cb43df81e6a/image.png?t=1793103653)

The agent finds the tests and requests permission to run a test command.

Once the local setup responds, exit OpenCode. Keep Ollama running in Terminal B.

```
/exit
```

## **Turning off the internet**

Turn Wi-Fi off and disconnect Ethernet or phone tethering if either is connected. Localhost still works because the request stays inside the Mac.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/5655a59f-c954-4807-b0b2-f48ed55ef5ed/image.png?t=1793103653)

Wi-Fi switched off for the connectivity check.

In Terminal A, check Ollama’s local API.

```
curl --noproxy '*' --fail http://127.0.0.1:11434/api/tags
```

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/6d3e3479-576b-4629-9c88-12e32d369c8a/image.png?t=1793103653)

Ollama still responds and lists the downloaded qwen3.5:9b model.

Then try an external address.

```
curl --noproxy '*' --connect-timeout 5 --max-time 10 --head https://example.com
```

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/9ae66f55-1b92-413a-ae3e-ac22e4041cab/image.png?t=1793103653)

The external request fails with *Could not resolve host*.

These checks establish what happened at the time of capture: the local service responded and the external lookup failed. Keep all external connections disconnected while running the coding task.

## **Fixing the bug offline**

Start OpenCode again in Terminal A using the same direct command.

```
.tools/node_modules/.bin/opencode --pure
```

Ask the agent to fix the bug.

```
Fix the bug in expense_total.py so the existing tests pass. Don't change the tests or install anything. Run the tests and explain your fix.
```

The recording shows Qwen proposing a one-line change, requesting edit permission and running the tests. The change is small, so it is easy to inspect.

```
total = Decimal(amount)   # before
```

```
total += Decimal(amount)  # after
```

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/e928a829-cbfe-455f-8b77-e0979dc3b42a/image.jpeg?t=1793103653)

Recorded edit and test cycle. Qwen proposes the fix, requests approval and runs five tests successfully.

After the agent finishes, exit OpenCode before running your own checks.

```
/exit
```

We then ran the tests ourselves in Terminal A.

```
python3 -m unittest -v
```

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/9051e1c6-5c39-4bd9-8cb4-4a4022ed802d/image.png?t=1793103653)

The manual check shows all five tests passing.

The saved source contains total += Decimal(amount), and the test file is unchanged from the starter. The fix now accumulates each amount. Running the sample program should return 25.50.

## **Two more coding tasks to try**

I’ve included two more exercises in the companion project: **a CSV report and a log analyzer**. I haven’t completed their offline agent runs yet, so the results shown above are from the expense example. These are the next tasks to try with the same setup.

The CSV report example starts with an unfinished function. Ask OpenCode to group rows by product, calculate quantities and revenue, and produce a readable report. It has sample data and seven tests covering grouping, decimal arithmetic, ordering and empty input.

Complete *examples/csv-report/csv\_report.py* using its README. Keep the tests unchanged and don't install anything. Run this example's tests and explain what you changed.

The log analyzer already runs but has bugs in error counting and selecting the slowest request. Its seven tests also cover malformed lines and empty input.

Fix *examples/log-analyzer/log\_analyzer.py* using its README. Keep the tests unchanged and don't install anything. Run its tests and explain your fix.

Each walkthrough contains the exact test commands and expected results. Run the exercises with the same local setup, then inspect the changes and test output.

## **What helps when the internet is unavailable**

- **Prepare the project as well as the model**. Download its dependencies and keep the documentation you will need on disk. Our examples use Python’s standard library, so their tests do not need package downloads.

- **Launch the exact setup once while online**. Use the same command, model and configuration for the offline attempt, so that required runtime components are already available.

- **Give the agent the project’s test command**. Ours initially proposed pytest even though this example uses unittest. A clear test command reduces unnecessary detours.

- **Keep the task focused and check the result**. Ask for one bug fix or a small feature, review the diff, and run the tests yourself. Keep the original tests so that a passing result still checks the intended behavior.

- **Watch memory pressure**. The model download is only part of the memory cost. A large context and other open apps can reduce the room available to the model.

- **Record the attempts that need help too**. A useful walkthrough should show permission prompts, retries and failures, as well as the final passing output.

## **What I learned from trying this offline**

Seeing the tests pass after turning Wi-Fi off answered the first question I had: **I could use a coding agent to read a local file, make a change and run checks without an internet connection.** I still reviewed the edit and ran the original tests myself. Keeping those tests unchanged gave me a useful way to check the result.

I’d start with **small, self-contained tasks** when using this setup during an outage or a journey. The project and everything it needs have to be on the laptop before the connection disappears. This one-line fix doesn’t tell me how well the model would handle a large repository or an unfamiliar framework. The CSV report is the next useful check: it asks the agent to complete a function, so there is more to inspect than a one-line repair.
