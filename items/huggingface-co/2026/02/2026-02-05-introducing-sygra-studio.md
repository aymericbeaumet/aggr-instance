---
title: Introducing SyGra Studio
link: https://huggingface.co/blog/ServiceNow-AI/sygra-studio
source: huggingface-co
published: 2026-02-05T16:52:28Z
updated: 2026-02-05T16:52:28Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2026-02-05-introducing-sygra-studio.html
---

[SyGra 2.0.0](https://huggingface.co/blog/ServiceNow-AI/sygra-v2) introduces **Studio**, an interactive environment that turns synthetic data generation into a transparent, visual craft. Instead of juggling YAML files and terminals, you compose flows directly on the canvas, preview datasets before committing, tune prompts with inline variable hints, and watch executions stream live—all from a single pane. Under the hood it’s the same platform, so everything you do visually generates the corresponding SyGra compatible graph config and task executor scripts.

### What Studio lets you do

1. Configure and validate models with guided forms (OpenAI, Azure OpenAI, Ollama, Vertex, Bedrock, vLLM, custom endpoints).
2. Connect Hugging Face, file-system, or ServiceNow data sources and preview rows before execution.
3. Configure nodes by selecting models, writing prompts (with auto-suggested variables), and defining outputs or structured schemas.
4. Design downstream outputs using shared state variables and Pydantic-powered mappings.
5. Execute flows end-to-end and review generated results instantly with node-level progress.
6. Debug with inline logs, breakpoints, Monaco-backed code editors, and auto-saved drafts.
7. Monitor per-run token cost, latency, and guardrail outcomes with execution history stored in `.executions/`.

Let’s walk through this experience step by step.

* * *

## Step 1: Configure the data source

Open Studio, click **Create Flow**, and Start/End nodes appear automatically. Before adding anything else:

- Choose a connector (Hugging Face, disk, or ServiceNow).
- Enter parameters like `repo_id`, split, or file path, then click **Preview** to fetch sample rows.
- Column names immediately become state variables (e.g., `{prompt}`, `{genre}`), so you know exactly what can be referenced inside prompts and processors.

Once validated, Studio keeps the configuration in sync and pipes those variables throughout the flow—no manual wiring or guesswork.

* * *

## Step 2: Build the flow visually

Drag the blocks you need from the palette. For a story-generation pipeline:

1. Drop an **LLM node** named “Story Generator,” select a configured model (say, `gpt-4o-mini`), write the prompt, and store the result in `story_body`.
2. Add a second **LLM node** named “Story Summarizer,” reference `{story_body}` inside the prompt, and output to `story_summary`.
3. Toggle structured outputs, attach tools, or add Lambda/Subgraph nodes if you need reusable logic or branching behavior.

Studio’s detail panel keeps everything in context—model parameters, prompt editor, tool configuration, pre/post-process code, and even multi-LLM settings if you want parallel generations. Typing `{` inside a prompt surfaces every available state variable instantly.

* * *

## Step 3: Review and run

Open the **Code Panel** to inspect the exact YAML/JSON Studio is generating. This is the same artifact written to `tasks/examples/`, so what you see is what gets committed.

When you’re ready to execute:

- Click **Run Workflow**.
- Choose record counts, batch sizes, retry behavior etc.
- Hit **Run** and watch the Execution panel stream node status, token usage, latency, and cost in real time. Detailed logs provide observability and make debugging effortless. All executions are written to `.executions/runs/*.json`.

After the run, download outputs, compare against prior executions, get metadata of latency and usage details.

## See it in action!

* * *

## Running Existing Workflows

### Run the Glaive Code Assistant workflow

SyGra Studio can also execute existing workflow in the `tasks`. For example, in the `tasks/examples/glaive_code_assistant/` [workflow](https://github.com/ServiceNow/SyGra/tree/v2.0.0/tasks/examples/glaive_code_assistant) — it ingests the `glaiveai/glaive-code-assistant-v2` [dataset](https://huggingface.co/datasets/glaiveai/glaive-code-assistant-v2), drafts answers, critiques them, and loops until the critique returns “NO MORE FEEDBACK.”

Inside Studio you’ll notice:

1. **Canvas layout** – two LLM nodes (`generate_answer` and `critique_answer`) linked by a conditional edge that either routes back for more revisions or exits to **END** when the critique is satisfied.
2. **Tunable inputs** – the Run modal lets you switch dataset splits, adjust batch sizes, cap records, or tweak temperatures without touching YAML.
3. **Observable execution** – watch both nodes light up in sequence, inspect intermediate critiques, and monitor status in real time.
4. **Generated outputs** – synthetic data is generated, ready for model training, evaluation pipelines or annotation tools.

* * *

## Get started

```bash
git clone https://github.com/ServiceNow/SyGra.git
cd SyGra && make studio
```

- **Docs**: [https://servicenow.github.io/SyGra/](https://servicenow.github.io/SyGra/)
- **Studio Docs**: [https://servicenow.github.io/SyGra/getting_started/create_task_ui/](https://servicenow.github.io/SyGra/getting_started/create_task_ui/)
- **Example config**: [`tasks/examples/glaive_code_assistant/graph_config.yaml`](https://github.com/ServiceNow/SyGra/blob/v2.0.0/tasks/examples/glaive_code_assistant/graph_config.yaml)

SyGra Studio turns synthetic data workflows into a visual, user friendly experience. Configure once, build with confidence, run with full observability, generate the data without ever leaving the canvas.
