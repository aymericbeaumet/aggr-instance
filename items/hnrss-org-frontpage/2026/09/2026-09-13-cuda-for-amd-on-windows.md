---
title: CUDA for AMD on Windows
link: https://github.com/Speedstu/CUDA-for-AMD-Windows
source: hnrss-org-frontpage
published: 2026-09-13T14:25:13Z
updated: 2026-09-13T14:25:13Z
first_seen: 2026-09-13T21:37:49.003143518Z
authors:
- chiassedu80
summary: 'Article URL: https://github.com/Speedstu/CUDA-for-AMD-Windows Comments URL: https://news.ycombinator.com/item?id=49684356 Points: 112 # Comments: 61'
content: extracted
html: 2026-09-13-cuda-for-amd-on-windows.html
preview:
  file: 2026-09-13-cuda-for-amd-on-windows.preview-52e68e8c10b3.webp
  width: 256
  height: 128
  alt: Run CUDA-targeted Windows applications on AMD GPUs with ZLUDA + ROCm/HIP. - Speedstu/CUDA-for-AMD-Windows
  color: '#e8e9eb'
images:
- source: https://opengraph.githubassets.com/f26e3b67e3db290248f8bb8408487b6b79db9c37d5e0c42b4619cfc8dd024651/Speedstu/CUDA-for-AMD-Windows
  original:
    file: 2026-09-13-cuda-for-amd-on-windows.image-d27fa89bb07f.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-13-cuda-for-amd-on-windows.image-39039f81f6be.webp
    width: 48
    height: 24
  - file: 2026-09-13-cuda-for-amd-on-windows.image-aea980309e26.webp
    width: 320
    height: 160
  - file: 2026-09-13-cuda-for-amd-on-windows.image-55f23cb0591c.webp
    width: 640
    height: 320
  - file: 2026-09-13-cuda-for-amd-on-windows.image-9629f1f31f57.webp
    width: 960
    height: 480
  - file: 2026-09-13-cuda-for-amd-on-windows.image-ba143f930544.webp
    width: 1200
    height: 600
  color: '#fefefe'
---

**WORKING REPRODUCIBLE STACK IS NOW UPLOADED.**

Run CUDA-targeted Windows applications on AMD GPUs through ZLUDA + ROCm/HIP.

[![Windows](https://camo.githubusercontent.com/bda49ca2aab63a7a686fd37e900fe66c7f12aac63dcf0b1347edd859524172a9/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f706c6174666f726d2d57696e646f77732532307836342d353535353535)](https://github.com/Speedstu/CUDA-for-AMD-Windows) [![AMD](https://camo.githubusercontent.com/9254941c8b7da2ae955349f1ac71864ac36a96cbc9736d0b18b24a743108b539/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4750552d414d44253230526164656f6e2d353535353535)](https://github.com/Speedstu/CUDA-for-AMD-Windows) [![verify](https://github.com/Speedstu/CUDA-for-AMD-Windows/actions/workflows/verify.yml/badge.svg)](https://github.com/Speedstu/CUDA-for-AMD-Windows/actions/workflows/verify.yml)

A reproducible Windows CUDA compatibility setup built around **ZLUDA + AMD HIP/ROCm**. It is intended for CUDA-facing compute applications, including workloads that use CUDA-enabled LibTorch.

Important

**Validated hardware is currently AMD Radeon RX 9060 XT (`gfx1200`) only.** Other AMD GPUs are candidates, not guaranteed working devices. If you test another card, please open a [GPU compatibility report](https://github.com/Speedstu/CUDA-for-AMD-Windows/issues/new?template=gpu-compatibility.yml), whether it works or fails.

## Verified today

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#verified-today)

The public, upstream-only path has been tested without any private/recovered DLLs:

- ZLUDA `v6-preview.69` from the official ZLUDA release
- AMD HIP SDK `6.4`
- LibTorch `2.3.0 + cu118`
- RX 9060 XT / `gfx1200`
- `nvcuda`, cuBLAS, cuBLASLt, cuSPARSE and cuFFT all pass `cuda_check`
- a real **2,216,347-parameter PPO network completed forward/inference, PPO learning and optimizer work on the CUDA-facing device**
- one clean validation iteration completed **65,536 timesteps** using the runtime produced by this repository

That integration test used the same CUDA-facing LibTorch training workload that originally motivated this project. See [`docs/VALIDATION.md`](https://github.com/Speedstu/CUDA-for-AMD-Windows/blob/main/docs/VALIDATION.md).

This does **not** mean every CUDA program or AI model works. CUDA API/library coverage is workload-dependent.

## How it works

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#how-it-works)

```
CUDA-targeted Windows application
              |
            ZLUDA
              |
 cuBLAS / cuSPARSE / cuFFT compatibility
              |
 rocBLAS / hipBLASLt / rocSPARSE / HIP
              |
           AMD GPU
```

## Install

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#install)

### 1\. Install the AMD prerequisites

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#1-install-the-amd-prerequisites)

Install a current AMD GPU driver and the **AMD HIP SDK for Windows including HIP Libraries**.

The validated reference uses HIP SDK 6.4. Newer versions may work but should be treated as unverified until reported.

AMD Windows HIP SDK guide: [https://rocm.docs.amd.com/projects/install-on-windows/en/docs-6.4.2/index.html](https://rocm.docs.amd.com/projects/install-on-windows/en/docs-6.4.2/index.html)

### 2\. Clone and run the installer

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#2-clone-and-run-the-installer)

```
git clone https://github.com/Speedstu/CUDA-for-AMD-Windows.git
cd CUDA-for-AMD-Windows
powershell -ExecutionPolicy Bypass -File .\scripts\install.ps1
```

`install.ps1` will:

1. detect the AMD GPU and native `gfxXXXX` target;
2. verify the AMD driver/HIP SDK and required math libraries;
3. download the pinned official ZLUDA Windows build;
4. download LibTorch `2.3.0+cu118` (about 2.66 GB);
5. verify the downloaded SHA-256 hashes;
6. generate `.runtime\runtime-config.json` and `.runtime\gpu-report.json`;
7. run ZLUDA's `cuda_check.exe` against the installed AMD stack.

If you do not need LibTorch:

```
.\scripts\install.ps1 -SkipLibTorch
```

## Run a CUDA-targeted application

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#run-a-cuda-targeted-application)

```
.\scripts\run-zluda.ps1 -Program C:\path\to\app.exe
```

The launcher stages the required ZLUDA compatibility DLLs beside the target application and sets the HIP/ROCm runtime paths for that run.

You can also stage without launching:

```
.\scripts\stage-runtime.ps1 -TargetDir C:\path\to\your-app
```

## Diagnose a machine

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#diagnose-a-machine)

```
.\scripts\doctor.ps1
.\scripts\gpu-scan.ps1
.\scripts\test-runtime.ps1
```

The GPU scanner records the model, `gfx` architecture, driver and HIP information. It does not intentionally collect usernames, tokens or user files.

Example on the validated machine:

```
AMD Radeon RX 9060 XT -> gfx1200 -> RDNA4 -> validated-reference
```

## Current GPU status

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#current-gpu-status)

| GPU               | Target    | Project status        |
| ----------------- | --------- | --------------------- |
| Radeon RX 9060 XT | `gfx1200` | ✅ validated reference |

The scanner recognizes other Windows HIP architecture families and marks them as **unverified candidates** rather than claiming support. Detection is not proof that a workload runs.

AMD's current Windows hardware table: [https://rocm.docs.amd.com/projects/install-on-windows/en/latest/reference/system-requirements.html](https://rocm.docs.amd.com/projects/install-on-windows/en/latest/reference/system-requirements.html)

## Runtime coverage on the validated setup

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#runtime-coverage-on-the-validated-setup)

Current upstream runtime check:

| CUDA-facing component  | Result                                                   |
| ---------------------- | -------------------------------------------------------- |
| CUDA driver / `nvcuda` | ✅                                                        |
| cuBLAS                 | ✅ via rocBLAS                                            |
| cuBLASLt               | ✅ via hipBLASLt                                          |
| cuSPARSE               | ✅ via rocSPARSE                                          |
| cuFFT                  | ✅                                                        |
| cuDNN                  | ⚠️ unavailable with the validated stable Windows HIP SDK |

The stable Windows HIP SDK does not ship the full ROCm AI-library stack such as MIOpen, so convolution-heavy software that requires cuDNN can need a newer/nightly HIP stack or additional work. Dense/GEMM-heavy LibTorch training does not necessarily require cuDNN; the validated PPO workload completed without it.

## Performance

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#performance)

A controlled 2026-09-13 A/B ran **10 iterations per runtime** on the same RX 9060 XT PPO workload. After discarding the first iteration of each trial as warmup, the public upstream path reached **13,278 median overall SPS** versus **12,876** for the recovered custom overlay. In this workload the custom overlay was about **3.03% slower**, so upstream remains the default.

Historical tuned runs used a different training configuration and reached roughly **70k–109k overall steps/s**. See [`docs/BENCHMARKS.md`](https://github.com/Speedstu/CUDA-for-AMD-Windows/blob/main/docs/BENCHMARKS.md) for methodology and raw data.

## Optional historical custom overlay

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#optional-historical-custom-overlay)

The original development environment also experimented with a custom cuBLAS/cuBLASLt/HIP overlay. It is **not required** for the validated public path and, based on the controlled A/B above, is not currently a performance win for the reference PPO workload.

The recovered DLLs remain fingerprinted in `manifests/recovered-artifacts.sha256`. They are not published as binary blobs because the original custom wrapper source/provenance is incomplete and the recovered HIP runtime contains third-party AMD binaries. See [`docs/CUSTOM_OVERLAY.md`](https://github.com/Speedstu/CUDA-for-AMD-Windows/blob/main/docs/CUSTOM_OVERLAY.md).

## Found a bug or tested another GPU?

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#found-a-bug-or-tested-another-gpu)

Please publish an issue. Failed tests are useful too.

```
.\scripts\gpu-scan.ps1 -OutputPath .\gpu-report.json
.\scripts\test-runtime.ps1
```

Then open a [GPU compatibility report](https://github.com/Speedstu/CUDA-for-AMD-Windows/issues/new?template=gpu-compatibility.yml) and include the application, result and first useful error/output.

## Repository layout

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#repository-layout)

```
scripts/              install, diagnostics, scanner, staging and launcher
manifests/            pinned versions, hashes and GPU architecture metadata
docs/                 validation, architecture, benchmarks and troubleshooting
examples/             integration/reference snippets
.runtime/             generated dependencies and reports; ignored by Git
local-artifacts/      local archival files; ignored by Git
```

## Limitations

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#limitations)

- Only RX 9060 XT / `gfx1200` is currently validated by this project.
- ZLUDA is not a complete CUDA implementation.
- Windows exposes only a subset of the full ROCm ecosystem.
- cuDNN/MIOpen is not available in the validated stable HIP SDK path.
- NCCL, TensorRT, unsupported PTX behavior and some custom CUDA extensions may fail.
- `ZLUDA_CC=8.6` is a CUDA-facing compatibility value, not the AMD GPU architecture.

## License and third-party software

[](https://github.com/Speedstu/CUDA-for-AMD-Windows#license-and-third-party-software)

Project-owned scripts and documentation are MIT licensed. ZLUDA, AMD ROCm/HIP, NVIDIA CUDA components and PyTorch/LibTorch retain their own upstream licenses. See [`THIRD_PARTY_NOTICES.md`](https://github.com/Speedstu/CUDA-for-AMD-Windows/blob/main/THIRD_PARTY_NOTICES.md).
