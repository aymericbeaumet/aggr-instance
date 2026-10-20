---
title: Google open-sources TorchTPU, a native PyTorch back end for TPUs
link: https://github.com/google-pytorch/torch_tpu
source: hnrss-org
published: 2026-10-20T16:40:24Z
updated: 2026-10-20T16:40:24Z
first_seen: 2026-10-20T21:31:53.761078291Z
authors:
- ilreb
content: extracted
html: 2026-10-20-google-open-sources-torchtpu-a-native-pytorch-back-end-for.html
remote_preview:
  url: https://opengraph.githubassets.com/cd86c84e93f7b104a4d7723deb52a1b58d9418986b422db17cdc92ee64b72b49/google-pytorch/torch_tpu
  alt: Contribute to google-pytorch/torch_tpu development by creating an account on GitHub.
---

[![TorchTPU Logo](https://github.com/google-pytorch/torch_tpu/raw/main/docs/images/torchtpu_logo.avif)](https://github.com/google-pytorch/torch_tpu/blob/main/docs/images/torchtpu_logo.avif)

## Description

This project provides a PyTorch backend for Google's Tensor Processing Units (TPUs), enabling users to run PyTorch models and operations on TPU devices. It includes custom ATen kernels for various PyTorch operations, a compilation cache for optimizing execution, and examples for distributed training and model inference.

## Features

- PyTorch ATen kernel implementation for TPU.
- Compilation cache for optimized execution of XLA computations.
- Support for distributed training (Data Parallelism, Tensor Parallelism).
- Examples for various models (Llama, Qwen, ResNet, DLRM, minGPT, UNet).
- Integration with `torch.compile()` for graph mode execution.
- Utilities for debugging and benchmarking.

## User Guide and Interactive Notebooks

A user guide is available at [https://torchtpu.readthedocs.io/](https://torchtpu.readthedocs.io/). This is a static version of the interactive notebooks. They are available in the docs folder.

The notebooks are Marimo (similar to Jupyter). They can be run with `marimo edit file.py` and then you can access them in your browser. You will need to be running this from a TPU VM, a v6e GCE instance is easiest.

## Installation

The same instructions are available in the user guide linked above.

TorchTPU's Python version support policy matches [PyTorch's Release Compatibility Matrix](https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-compatibility-matrix). Currently, **Python 3.11+** is supported. See the [Python Version Support Policy](https://github.com/google-pytorch/torch_tpu/blob/main/docs/developers/python_support_policy.md) for more details.

First create a Python virtual environment:

```shell
python -m venv .venv
source .venv/bin/activate
```

or, with `uv`

```shell
uv venv .venv
source .venv/bin/activate
```

### Install via pip

Install the latest public release ([0.2.0 on PyPI](https://pypi.org/project/torch-tpu/0.2.0/)) (for `uv` prepend `uv` to all `pip` commands):

```shell
pip install torch_tpu==0.2.0 --extra-index-url https://download.pytorch.org/whl/cpu
```

### Build from source

Wheels can be built via:

```shell
bazel build --config=wheel_common //ci/wheel:torch_tpu_wheel
```

All OSS Bazel builds default to optimized mode (`-c opt`) automatically via `.bazelrc`. Unoptimized builds (`-c fastbuild`, `-c dbg`) can degrade performance and are not used for wheel builds. If you are developing or debugging C++ code and explicitly want an unoptimized wheel, pass `--//:allow_unoptimized_wheel=True`:

```shell
bazel build -c dbg --config=wheel_common //ci/wheel:torch_tpu_wheel \
    --//:allow_unoptimized_wheel=True
```

`--config=wheel_common` is required, and the build fails at build time without it. The config sets `--//:wheel_build=True`, which routes the shared XLA/MLIR backend through the `torch_version`-reset transition so that pywrap factors it into a single `libxla_base.so`; built without the flag, every per-version common ships its own copy of the backend and the resulting wheel aborts on `import torch` with duplicate static registrations.

`wheel_common` also turns on the Remote Build Execution (RBE) remote cache and remote executor. If you do not have RBE credentials, append `--config=no_rbe` to strip those flags back out:

```shell
bazel build --config=wheel_common //ci/wheel:torch_tpu_wheel --config=no_rbe
```

The order matters here: `--config=no_rbe` has to come after `--config=wheel_common`, because the later config is the one that wins for the remote-cache and remote-executor flags they both set.

Install wheel via:

```shell
cd <path_to_repo>
python -m venv .venv; source .venv/bin/activate
# The extra-index-url provides the CPU version of torch rather than the default CUDA build.
pip install ../bazel-bin/ci/wheel/*.whl --extra-index-url https://download.pytorch.org/whl/cpu
```

Note that installing without `--extra-index-url https://download.pytorch.org/whl/cpu` will install the CUDA version of `torch`; pass `--extra-index-url` as shown above or install the CPU build separately.

## Getting Started

Some pointers to get you started:

- [PyTorch: Official PyTorch documentation](https://github.com/pytorch/pytorch)
- [Tutorials and examples: easy to understand PyTorch code using TorchTPU](https://torchtpu.readthedocs.io/)
- [Public APIs: the TorchTPU APIs that you can use](https://github.com/google-pytorch/torch_tpu/blob/main/docs/public_apis.md)

## Dependencies

The project uses Bazel for dependency management. Key Python dependencies are:

TBD

## Bazel setup

This project uses Bazel for building and dependency management.

To install Bazel, we actually install Bazelisk, which is a transparent wrapper that handles downloading and installing Bazel itself.

There are several ways to install Bazelisk. Here are some common ways:

- If you have Go installed:

  ```
  go install github.com/bazelbuild/bazelisk@latest
  ```

- Download the binary directly:

  ```
  wget -O ~/bin/bazel https://github.com/bazelbuild/bazelisk/releases/download/v1.27.0/bazelisk-linux-amd64
  ```

See the [Bazelisk README](https://github.com/bazelbuild/bazelisk/blob/master/README.md) for more install instructions

However you install it, add it's location to `PATH`.

To verify the install, run `bazel info` in the repo's root directory:

```
bazel info
```

It should print information about the repo without errors.

## VSCode `clangd` Setup

For C++ code navigation, it is recommended to use VSCode with [`clangd` extension](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd). Relying on a [compilation database](https://clang.llvm.org/docs/JSONCompilationDatabase.html) generated from Bazel action graph, [clangd](https://clangd.llvm.org/) is able to enrich the editor with various smart features, including code completions, go-to definitions, *etc.*

1. Install VSCode extensions via UI or the following command:

   ```shell
   code --install-extension llvm-vs-code-extensions.vscode-clangd
   # Uninstall Microsoft C++ extensions to avoid interference.
   code --uninstall-extension ms-vscode.cpptools
   ```

2. Generate the initial `compile_commands.json`:

   ```shell
   tools/setup_clangd.py
   ```

It may take a while (> 30 minutes) for the first run to make a full build, but reruns will be incremental and cheap. If you have run `bazel build` recently, generated files already exist so you can pass `--no-build` to skip waiting for heavy builds (note that on a fresh workspace, skipping the build may prevent `clangd` from resolving generated files). See more options in the [script](https://github.com/google-pytorch/torch_tpu/blob/main/tools/setup_clangd.py) file-level comments.

Alternatively, you can invoke the script via `nox` (arguments after `--` are forwarded):

```shell
nox -s refresh_compile_commands -- --no-build
nox -s refresh_compile_commands -- //src/torch_tpu/ops/...
```

## Contributing

We welcome contributions to the PyTorch on TPU project! Please see our [CONTRIBUTING.md](https://github.com/google-pytorch/torch_tpu/blob/main/CONTRIBUTING.md) for guidelines on how to contribute, including signing our Contributor License Agreement (CLA) and code review processes.

## Star History

[![Star History Chart](https://camo.githubusercontent.com/5dd8f8b127ebb9d4b8e81870ade5b9dea014587bc270e5ff7c4984ca8bf88d43/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f63686172743f7265706f733d676f6f676c652d7079746f7263682f746f7263685f74707526747970653d64617465266c6567656e643d746f702d6c656674)](https://www.star-history.com/?repos=google-pytorch%2Ftorch_tpu&type=date&legend=top-left)

## License

This project is licensed under the Apache 2.0 License. See the [LICENSE](https://github.com/google-pytorch/torch_tpu/blob/main/LICENSE) file for details.
