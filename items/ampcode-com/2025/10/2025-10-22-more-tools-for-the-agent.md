---
title: More Tools for the Agent
link: https://ampcode.com/news/more-tools-for-the-agent
source: ampcode-com
published: 2025-10-22T00:00:00Z
updated: 2025-10-22T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'We shipped several changes to toolboxes that make them easier to use: You can now have multiple toolboxes in the AMP_TOOLBOX environment variable amp tools make is a new command to create a functional example tool in the right location amp tools show lets you inspect a tool amp tools use allows you to execute it And Amp itself can fill in all the details to make the tool work as you intended. If you don''t yet know what toolboxes are: a toolbox is a directory full of UNIX-style programs that provide custom tools to the agent locally. Conceptually, they sit between MCP servers and common CLI tools, but are less complex than MCP servers and easier for the agent to use than plain CLI tools. Let''s use an example to see how we can use amp tools make and amp tools use to create and run a new tool in a new toolbox. Let''s build a run_tests tool. Chances are that if you have used any coding agent before, you''ve probably seen it trying to run the your tests, but then stumble and having to guess a few times what the correct command invocation is. We can make (artificial) life for the agent easier by giving it a run_tests tool that will always use the correct command. First we create a .amp/tools directory in our repository and add it to the AMP_TOOLBOX variable, so Amp knows to look there for tools: # Create the directory mkdir -p .amp/tools # Add to AMP_TOOLBOX. Order matters: prefer tools in the repo over global tools export AMP_TOOLBOX="$PWD/.amp/tools:$HOME/.config/amp/tools" Next we run amp tools make to create a new tool, using a built-in template: amp tools make run_tests Which prints: Tool created at: /Users/dhamidi/w/amp-wip/.amp/tools/run_tests Inspect with: amp tools show tb__run_tests Execute with: amp tools use tb__run_tests Great! Now we could open .amp/tools/run_tests manually and edit it as needed, so it actually does what we want and runs the tests. But, hey, we can also tell Amp to inspect our package.json file, find the right test command, and adjust the tool template accordingly: amp --dangerously-allow-all -x ''Edit .amp/tools/run_tests to run the tests using pnpm test according to our package.json file. It should have an optional workspace parameter and an optional parameter for filtering by test name'' Now we can use amp tools show to inspect the tool and see what Amp has created: amp tools show tb__run_tests That prints the description of the tool (which the Amp agent will see) and the schema of the input parameters (which the agent will use to run the tool): # tb__run_tests (toolbox: /Users/dhamidi/w/amp-wip/.amp/tools/run_tests) You must use this tool to run tests instead of using the Bash tool. This tool runs tests using pnpm test with optional workspace filtering and test name filtering. # Schema - workspace (string): optional workspace name to run tests in (e.g., "core", "web", "server", "cli"). If not provided, runs tests in the root workspace - testName (string): optional test name filter to run specific tests (passed as -t argument to vitest) But before we now have the agent try it, we can use amp tools use to execute the tool to make sure it works: amp tools use --only output tb__run_tests --testName "writes a tool" Note how we specified --testName, one of the input parameters of the tool. If we run the command, we''ll see something like this: # ... Test Files 1 passed | 306 skipped (307) Tests 1 passed | 4181 skipped (4182) It works! And now, when you start Amp, it will automatically register this tool at startup, and use it to run the tests:'
content: extracted
html: 2025-10-22-more-tools-for-the-agent.html
---

We shipped several changes to [toolboxes](https://ampcode.com/manual#toolboxes) that make them easier to use:

- You can now have multiple toolboxes in the `AMP_TOOLBOX` environment variable
- `amp tools make` is a new command to create a functional example tool in the right location
- `amp tools show` lets you inspect a tool
- `amp tools use` allows you to execute it

And Amp itself can fill in all the details to make the tool work as you intended.

If you don't yet know what [toolboxes](https://ampcode.com/manual#toolboxes) are: a toolbox is a directory full of UNIX-style programs that provide custom tools to the agent locally. Conceptually, they sit between MCP servers and common CLI tools, but are less complex than MCP servers and easier for the agent to use than plain CLI tools.

Let's use an example to see how we can use `amp tools make` and `amp tools use` to create and run a new tool in a new toolbox.

Let's build a `run_tests` tool. Chances are that if you have used any coding agent before, you've probably seen it trying to run the your tests, but then stumble and having to guess a few times what the correct command invocation is. We can make (artificial) life for the agent easier by giving it a `run_tests` tool that will always use the correct command.

First we create a `.amp/tools` directory in our repository and add it to the `AMP_TOOLBOX` variable, so Amp knows to look there for tools:

```bash
# Create the directory
mkdir -p .amp/tools
# Add to AMP_TOOLBOX. Order matters: prefer tools in the repo over global tools
export AMP_TOOLBOX="$PWD/.amp/tools:$HOME/.config/amp/tools"
```

Next we run `amp tools make` to create a new tool, using a built-in template:

```bash
amp tools make run_tests
```

Which prints:

```
Tool created at: /Users/dhamidi/w/amp-wip/.amp/tools/run_tests
Inspect with: amp tools show tb__run_tests
Execute with: amp tools use tb__run_tests
```

Great! Now we could open `.amp/tools/run_tests` manually and edit it as needed, so it actually does what we want and runs the tests. But, hey, we can also tell Amp to inspect our `package.json` file, find the right test command, and adjust the tool template accordingly:

```bash
amp --dangerously-allow-all -x 'Edit .amp/tools/run_tests to run the tests
using pnpm test according to our package.json file.
It should have an optional workspace parameter
and an optional parameter for filtering by test name'
```

Now we can use `amp tools show` to inspect the tool and see what Amp has created:

```
amp tools show tb__run_tests
```

That prints the description of the tool (which the Amp agent will see) and the schema of the input parameters (which the agent will use to run the tool):

```
# tb__run_tests (toolbox: /Users/dhamidi/w/amp-wip/.amp/tools/run_tests)

You must use this tool to run tests instead of using the Bash tool.
This tool runs tests using pnpm test with optional workspace filtering
and test name filtering.

# Schema

- workspace (string): optional workspace name to run tests in (e.g., "core", "web", "server", "cli"). If not provided, runs tests in the root workspace
- testName (string): optional test name filter to run specific tests (passed as -t argument to vitest)
```

But before we now have the agent try it, we can use `amp tools use` to execute the tool to make sure it works:

```
amp tools use --only output tb__run_tests --testName "writes a tool"
```

Note how we specified `--testName`, one of the input parameters of the tool. If we run the command, we'll see something like this:

```
# ...
 Test Files  1 passed | 306 skipped (307)
      Tests  1 passed | 4181 skipped (4182)
```

It works! And now, when you start Amp, it will automatically register this tool at startup, and use it to run the tests:
