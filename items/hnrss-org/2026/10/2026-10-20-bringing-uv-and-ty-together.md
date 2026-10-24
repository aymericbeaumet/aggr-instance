---
title: Bringing uv and ty together
link: https://astral.sh/blog/uv-ty-integration
source: hnrss-org
published: 2026-10-20T21:30:56Z
updated: 2026-10-20T21:30:56Z
first_seen: 2026-10-24T09:25:49.691482789Z
authors:
- Ravencentric
content: extracted
html: 2026-10-20-bringing-uv-and-ty-together.html
remote_preview:
  url: https://astral.sh/static/OpenGraph/Uv_Ty_Integration.png
---

**TL;DR:** Experimental uv-ty integration is now available, enabling first-class support for Python scripts (on the CLI and in the editor) and a unified `uv check` command.

This is a step towards an integrated Astral toolchain, one that uses a shared understanding of your project to enable entirely new capabilities.

* * *

At Astral, we build high-performance tools for the Python ecosystem: [uv](https://github.com/astral-sh/uv), our package manager; [Ruff](https://github.com/astral-sh/ruff), our linter and formatter; and [ty](https://github.com/astral-sh/ty), our type checker and language server.

Historically, each Astral tool has focused on making a different component of Python development more productive. But we see an opportunity to further Python by unifying traditionally separate capabilities into an integrated toolchain.

Our vision for the Astral toolchain is simple: while each tool should be individually usable, useful, and interoperable with the rest of the ecosystem, they should be even more capable when used together.

Today, we’re announcing an experimental **integration between uv and ty**, available in preview. uv understands your Python project’s environment and dependencies, while ty understands your code. By combining the two, we can unlock new functionality beyond what either tool can provide individually.

## First-class support for Python scripts

[PEP 723](https://peps.python.org/pep-0723/) enables Python scripts to declare their dependencies and Python version requirements through inline metadata. uv already uses this metadata to prepare an environment when running a script. Now, ty can use this metadata to understand your scripts.

On both the command line and in the editor, ty now uses inline metadata to discover scripts throughout your project and leverages uv to prepare an environment for each script. These environments allow ty to check your scripts against their declared requirements.

Scripts can have different requirements within the same repository. Each script is checked against its own dependency declarations, even if those dependencies or Python requirements differ from the surrounding project.

Declare dependencies inline with PEP 723, save to sync with uv, and use ty’s completions, hover information, and Go to Definition capabilities in your editor.

*Declare dependencies inline with PEP 723, save to sync with uv, and use ty’s completions, hover information, and Go to Definition capabilities in your editor.*

When the integration is enabled in your editor, ty automatically invokes uv to prepare the script’s environment in the background, which it uses to provide type checking, code navigation, and completions based on the script’s declared requirements.

Over time, we plan to enable script support by default, and enable automatic environment setup for standard Python projects.

## A unified checking command

`uv check` introduces a unified diagnostic interface to uv, starting with support for ty. Over time, we plan to support Ruff in `uv check` to unify linting and type checking into a single command.

Running `uv check` will automatically prepare your project’s environment, install the required dependencies, and check your project with ty. It “just works.” (If you never run `uv check`, nothing changes for your uv experience; the ty binary is installed separately on first invocation.)

With `uv check`, you can check individual packages, or an entire workspace. In a workspace, use `uv check --package <name>` to select an individual package, or `uv check --all-packages` to check every package. uv uses your workspace configuration to determine what to check, so the command stays the same as your workspace grows.

Sync your project’s dependencies and type-check your code with a single command.

*Sync your project’s dependencies and type-check your code with a single command.*

Standalone PEP 723 scripts have independent environments and are checked separately. Use `uv check --script example.py` to check a script using its declared requirements.

`uv check` automatically uses the ty version recorded in your project’s lockfile. Commit your project configuration and lockfile, and developers and CI running `uv check` will share the same ty version without separately maintained pins.

The [ty pre-commit hook](https://github.com/astral-sh/ty-pre-commit) also uses `uv check` under the hood to run ty before each commit over your Python project. By using `uv check`, the hook automatically sets up the environment for ty from your project’s dependency declarations.

## How it works

The uv-ty integration is powered by [`uv workspace metadata`](https://docs.astral.sh/uv/reference/internals/metadata/), which gives ty a structured view of your project or script’s packages, dependencies, and Python environment. ty then uses this information to provide type checking and editor features that reflect the configured environment.

For PEP 723 scripts, ty invokes uv to prepare the script’s environment, which includes installing declared dependencies. As dependency installation can require execution of third-party Python code, automatic environment setup is currently opt-in.

To [try the integration](https://docs.astral.sh/ty/reference/editor-settings/#experimentaluseuv) for PEP 723 scripts in VS Code, [install uv](https://docs.astral.sh/uv/getting-started/installation/) and the [ty VS Code extension](https://marketplace.visualstudio.com/items?itemName=astral-sh.ty), then enable the uv integration in your VS Code `settings.json`.

```json
{
  "ty.experimental.useUv": "on"
}
```

You can also check your project and its PEP 723 scripts from the command line with:

```shell
TY_UV=scripts ty check
```

[Running `uv check`](https://docs.astral.sh/uv/reference/cli/#uv-check) enables the integration automatically and requires no additional configuration.

Support for PEP 723 scripts and the `uv check` command requires uv 0.12.3 or later, and ty 0.0.76 or later.

## Up next

This integration is experimental, and we’d love your feedback as we refine the experience.

While this initial support largely leverages uv’s environment capabilities to power ty execution, we’re also working on a new category of checks that combine uv’s knowledge of your project’s dependencies with ty’s understanding of your code (e.g., to detect unused dependencies), which we’ll cover in a future post.

And, in parallel, we’re continuing toward ty’s stable release.

Thank you for building with us!

*Thanks to [Aria Desires](https://github.com/Gankra), [Micha Reiser](https://github.com/MichaReiser), and [Zsolt Dollenstein](https://github.com/zsol) from the Astral team for implementing the features behind this integration.*
