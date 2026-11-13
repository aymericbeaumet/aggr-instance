---
title: 'llama.cpp v1.0.0 : A stronger foundation for the local AI ecosystem'
link: https://huggingface.co/blog/ggml-org/llama-cpp-v1
source: huggingface-co
published: 2026-11-13T14:05:22Z
updated: 2026-11-13T14:05:22Z
first_seen: 2026-11-13T15:39:08.702380659Z
content: extracted
html: 2026-11-13-llama-cpp-v1-0-0-a-stronger-foundation-for-the-local-ai.html
remote_preview:
  url: https://cdn-uploads.huggingface.co/production/uploads/63148d3b996c52bf0142cdbe/2PgU1GxpJX_LVORaP2RRy.png
---

llama.cpp makes it possible to run multi-modal LLMs across a wide range of hardware - from consumer-grade local machines to high-end enterprise servers. Through quantization, optimized backends, and a portable C/C++ implementation, we make AI inference accessible at every level of the software stack. We believe this is the most lightweight, versatile and sensible platform for building cross-platform and private AI products.

During the 3.5 years since its [inception](https://github.com/ggml-org/llama.cpp/commit/26c0846), the project has grown a lot and many downstream applications already depend on our work. We expect this trend to continue as local AI and privacy become increasingly important in our everyday lives. Every new integration means more developers depending on our library interfaces and therefore having a flexible but stable API is paramount.

Today we release libraries `v1.0.0`, highlighting our responsibility to those developers. Moving forward, both [llama.cpp](https://github.com/ggml-org/llama.cpp) and [ggml](https://github.com/ggml-org/ggml) will reflect API and ABI incompatibilities with major-version bumps. As adoption grows, we will be stricter about making those boundaries visible to downstream consumers.

### Releases

| Release                                                                                                    | Library    | Public header(s)                                                                    |
| ---------------------------------------------------------------------------------------------------------- | ---------- | ----------------------------------------------------------------------------------- |
| [llama.cpp v1.0.0](https://github.com/ggml-org/llama.cpp/releases/tag/v1.0.0) - LLM inference in C/C++    | `libllama` | [`llama.cpp/include/`](https://github.com/ggml-org/llama.cpp/blob/master/include/) |
| [ggml v1.0.0](https://github.com/ggml-org/ggml/releases/tag/v1.0.0) - Tensor library for machine learning | `libggml`  | [`ggml/include/`](https://github.com/ggml-org/llama.cpp/blob/master/ggml/include/) |

### Semantic version update rules

The following rules apply to all [ggml-org](https://huggingface.co/ggml-org) projects using [semantic versioning](https://semver.org):

| Change type | Bump                                                                         |
| ----------- | ---------------------------------------------------------------------------- |
| **MAJOR**   | Breaking change to the public C API in `./include`                           |
| **MINOR**   | Backward-compatible feature, backend updates, model support, or API addition |
| **PATCH**   | Bug fix with no API change                                                   |

### Distribution

We are looking forward to expanding the ways in which `ggml-org` software is distributed and reaches developers and end-users. For more information about how to add `ggml-org` projects to your packaging and release pipelines, please check the following [discussion](https://github.com/ggml-org/ggml/discussions/1579).

| Channel                                                                      | Platforms                           |
| ---------------------------------------------------------------------------- | ----------------------------------- |
| [**Pre-release artifacts**](https://github.com/ggml-org/llama.cpp/releases) | Linux, macOS, Windows, Android, iOS |
| **Docker**                                                                   | Linux                               |
| **Debian**                                                                   | Linux                               |
| **Homebrew**                                                                 | macOS, Linux                        |
| **conda-forge**                                                              | Windows, macOS, Linux               |
| **MacPorts**                                                                 | macOS                               |
| **Winget**                                                                   | Windows                             |
| **XCFramework**                                                              | iOS, macOS                          |

### Development

The project has fostered an amazing open-source community with highly-skilled and talented contributors. With the rapid advance of AI capabilities in the field of software engineering, we continue to adapt and integrate LLMs in our developement cycle in a thoughtful and resonable manner. Maintaining a high quality codebase, staying in control of the architecture and understanding the implementation details will remain our top priorities.

To reinforce the faster pace of development we continue to invest more and more resources in improving the CI infrastructure behind the projects. Thanks to HF technologies such as [HF Buckets](https://huggingface.co/docs/huggingface_hub/en/guides/buckets) and [HF Jobs](https://huggingface.co/docs/huggingface_hub/en/guides/jobs) we are able to scale the capacity necessary to guarantee correctness for every change that lands on the `master` branches. With hardware support from NVIDIA, AMD, Qualcomm, Arm, IBM and Intel, we continue to enroll more self-hosted runners across all major hardware platforms.

### Backend and model coverage

The underlying machine learning engine of `llama.cpp` - `ggml` is designed to run on virtually any hardware in existence. This is achieved by implementing dedicated ggml backends. Each backend can be compiled as a dynamic library and optionally loaded at runtime.

A complete list of the currently implemented backends can be found [here](https://github.com/ggml-org/ggml/tree/master/src).

We are committed to provide continuous support of all model architectures. Currently, llama.cpp supports more than [150 architecture variants](https://github.com/ggml-org/llama.cpp/tree/master/src/models) as well as all mainstream speculative decoding methods.

### Thanks

The llama.cpp team at Hugging Face is thankful for the incredible support from the community. Special thanks to the [long-time maintainers](https://github.com/ggml-org/llama.cpp/blob/master/CODEOWNERS) of the project and all [contributors](https://github.com/ggml-org/llama.cpp/graphs/contributors?from=8%2F8%2F2026). This is a significant milestone which would not have been possible without our joint efforts!

### Additional resources

- [llama.cpp examples](https://github.com/ggml-org/llama.cpp/tree/master/examples)
- [ggml-org at Hugging Face](https://huggingface.co/ggml-org)
- [ggml-org at Github](https://github.com/ggml-org)
- Check out [Llama](https://llama.app/) desktop apps for MacOS and Windows, shipped with love by the team behind llama.cpp and Hugging Face.
