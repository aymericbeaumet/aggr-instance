---
title: 'Profiling in PyTorch (Part 3): Attention is all you profile'
link: https://huggingface.co/blog/torch-attention-profile
source: huggingface-co-blog
published: 2026-07-10T00:00:00Z
updated: 2026-07-10T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.html
preview:
  file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.preview-94b345cac640.webp
  width: 256
  height: 128
  color: '#d9e1d8'
images:
- source: https://huggingface.co/blog/assets/torch-attention-profile/thumbnail.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-166509e30885.png
    width: 1280
    height: 640
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-ae0ea5e4799a.webp
    width: 320
    height: 160
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-1b6836b7c41a.webp
    width: 640
    height: 320
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-568862071c40.webp
    width: 1280
    height: 640
  color: '#fdfdfd'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/profile-3-thumbnail.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-166509e30885.png
    width: 1280
    height: 640
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-ae0ea5e4799a.webp
    width: 320
    height: 160
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-1b6836b7c41a.webp
    width: 640
    height: 320
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-568862071c40.webp
    width: 1280
    height: 640
  color: '#fdfdfd'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-naive.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-1c93b1e8f285.png
    width: 9624
    height: 2036
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-f73be3a2ecc2.webp
    width: 320
    height: 68
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-05cf2123f23d.webp
    width: 640
    height: 135
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-07ea2fe284af.webp
    width: 960
    height: 203
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-861c945e279b.webp
    width: 1280
    height: 271
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-f6d6b34f15d6.webp
    width: 1600
    height: 338
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-e556d64aa1e9.webp
    width: 9624
    height: 2036
  color: '#f7f9f9'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/gpu-profile-naive.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-88b7c0c96d09.png
    width: 11440
    height: 2160
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-6ea4a93a82b0.webp
    width: 320
    height: 60
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-68d57b76fc6a.webp
    width: 640
    height: 121
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-26ead5407cde.webp
    width: 960
    height: 181
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-374baf683d33.webp
    width: 1280
    height: 242
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-06d3973c6b0b.webp
    width: 1600
    height: 302
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-5e52dffd9ad5.webp
    width: 11440
    height: 2160
  color: '#fcfcfc'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-naive.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-299e038519e7.png
    width: 10940
    height: 1440
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-fafe2e1729b0.webp
    width: 320
    height: 42
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-23ae32b6e0e7.webp
    width: 640
    height: 84
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-2cd0d49d1fbb.webp
    width: 960
    height: 126
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-401e3930bdbb.webp
    width: 1280
    height: 168
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-818028414b38.webp
    width: 1600
    height: 211
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-95d364be75ba.webp
    width: 10940
    height: 1440
  color: '#eaeced'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-inplace.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-f1f4f97ee696.png
    width: 13752
    height: 1920
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-33ef5d3aa31a.webp
    width: 320
    height: 45
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-3e65b3727b90.webp
    width: 640
    height: 89
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-259684b87bc7.webp
    width: 960
    height: 134
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-ebb9303fbe9c.webp
    width: 1280
    height: 179
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-a2675c95c2b4.webp
    width: 1600
    height: 223
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-10d390552a13.webp
    width: 13752
    height: 1920
  color: '#f5f8f9'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-inpace.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-868500813c49.png
    width: 10304
    height: 1428
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-c55145899ea7.webp
    width: 320
    height: 44
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-d0b8dd462196.webp
    width: 640
    height: 89
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-d461e3843e8e.webp
    width: 960
    height: 133
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-ba881728877d.webp
    width: 1280
    height: 177
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-41e4c9266627.webp
    width: 1600
    height: 222
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-d0407d5e51e4.webp
    width: 10304
    height: 1428
  color: '#cacacb'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/inplace-kernel-launches.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-6a5f7e1860f0.png
    width: 15372
    height: 1852
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-319755ee9738.webp
    width: 320
    height: 39
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-b20addc96a0e.webp
    width: 640
    height: 77
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-bad7fee48cc1.webp
    width: 960
    height: 116
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-12dfbf241dee.webp
    width: 1280
    height: 154
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-00f099b201a9.webp
    width: 1600
    height: 193
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-55bc52c839b0.webp
    width: 15372
    height: 1852
  color: '#f8f9f7'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/math-kernel-launches.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-c204a82f1ecd.png
    width: 14892
    height: 2288
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-c435eeba8b0e.webp
    width: 320
    height: 49
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-c9dc43a0076d.webp
    width: 640
    height: 98
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-b1bfee01a017.webp
    width: 960
    height: 147
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-19fb28b2f5b5.webp
    width: 1280
    height: 197
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-57ed9fcda3cd.webp
    width: 1600
    height: 246
  color: '#f8f9f9'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/tensor-core-kernels.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-31e396a49b5b.png
    width: 9540
    height: 2224
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-2bcc0d143395.webp
    width: 320
    height: 75
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-abf4de75eb4e.webp
    width: 640
    height: 149
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-e416b09c7119.webp
    width: 960
    height: 224
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-bd2030d67b09.webp
    width: 1280
    height: 298
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-39d07678a666.webp
    width: 1600
    height: 373
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-450dadf3be55.webp
    width: 9540
    height: 2224
  color: '#fbfcfc'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cuda-core-kernels.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-a75adea3ec70.png
    width: 9024
    height: 2664
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-c0519ad69167.webp
    width: 320
    height: 94
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-4af00045516c.webp
    width: 640
    height: 189
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-69206d7616d5.webp
    width: 960
    height: 283
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-4719df7520e2.webp
    width: 1280
    height: 378
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-5c36382560f4.webp
    width: 1600
    height: 472
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-cd0eebd082f7.webp
    width: 9024
    height: 2664
  color: '#fcfcfc'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/mask-math.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-78e68aed0080.png
    width: 15352
    height: 2584
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-b423136d9c0b.webp
    width: 320
    height: 54
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-c6cd45a6d174.webp
    width: 640
    height: 108
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-835e22718b3f.webp
    width: 960
    height: 162
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-e85b9d3f28e0.webp
    width: 1280
    height: 215
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-b454a6da8b28.webp
    width: 1600
    height: 269
  color: '#f8f9f7'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/safe-softmax-extra-kernels.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-25ceb56785b7.png
    width: 14184
    height: 2676
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-b9f25abf3a3c.webp
    width: 320
    height: 60
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-aae218601fd8.webp
    width: 640
    height: 121
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-064f743ab7e5.webp
    width: 960
    height: 181
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-fb161d5fc1e2.webp
    width: 1280
    height: 241
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-c73776dc3f6a.webp
    width: 1600
    height: 302
  color: '#f2ece9'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/efficient-backend.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-26decf7a03bb.png
    width: 3524
    height: 508
  color: '#f9fafa'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-backend.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-47f7abce6ac1.png
    width: 3720
    height: 558
  color: '#f9fafa'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-occupancy.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-79fb75c81f74.png
    width: 726
    height: 502
  color: '#fdfdfe'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-reg-count.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-17f2bda13ffd.png
    width: 826
    height: 522
  color: '#fefefe'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-backend.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-3babbffb9ec3.png
    width: 3724
    height: 552
  color: '#f8faf9'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-transpose.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-a09d85d09bfc.png
    width: 12172
    height: 2328
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-14611b04f2aa.webp
    width: 320
    height: 61
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-da87fb4b0b2a.webp
    width: 640
    height: 122
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-f256f663ebbd.webp
    width: 960
    height: 184
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-1c49e8645ae7.webp
    width: 1280
    height: 245
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-05170acbd147.webp
    width: 1600
    height: 306
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-fa9969674524.webp
    width: 12172
    height: 2328
  color: '#fefdfd'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/efficient-trasnpose.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-1800fca5c5c5.png
    width: 13436
    height: 2200
  variants:
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-2e9bdbf10701.webp
    width: 320
    height: 52
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-832420422110.webp
    width: 640
    height: 105
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-e706cdb2ea82.webp
    width: 960
    height: 157
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-97f0d5442948.webp
    width: 1280
    height: 210
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-97ca2d74fbe5.webp
    width: 1600
    height: 262
  - file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-d5194ded2f85.webp
    width: 13436
    height: 2200
  color: '#d6d7d7'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-launch.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-cbd19d725c5e.png
    width: 3240
    height: 560
  color: '#fbfcfc'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-footprint.png
  original:
    file: 2026-07-10-profiling-in-pytorch-part-3-attention-is-all-you-profile.image-9d75942f72d1.png
    width: 702
    height: 544
  color: '#fdfdfd'
---

[![Thumbnail of the blog post](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/profile-3-thumbnail.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/profile-3-thumbnail.png)

The series "Profiling in PyTorch" is meant to make you comfortable reading profiler traces and tables. In [Part 1](https://huggingface.co/blog/torch-profiler) we profiled basic math operations like addition and multiplication. We saw how the profiler table uncovers hotspots, and how the profiler trace shows the order in which an algorithm runs over time.

In [Part 2](https://huggingface.co/blog/torch-mlp-fusion) we wrapped that addition and multiplication into a torch linear layer. We then stacked several linear layers on top of each other (a multilayer perceptron) and profiled that. Along the way we also profiled fused and hand-tuned kernels.

From the perspective of the Transformer architecture, the next logical step for us to profile is yet another fundamental algorithm, attention. While being infamous for its quadratic-time complexity, many clever tricks exist to mitigate that issue and make it fast. Our goal here is not to cover every trick in detail. Instead, we want to see how each one looks different under the profiler.

> The scripts for this blog post live here: [`04_a_naive_attention.py`](https://huggingface.co/datasets/ariG23498/profiling-pytorch/blob/main/04_a_naive_attention.py), [`04_b_inplace_ops_attention.py`](https://huggingface.co/datasets/ariG23498/profiling-pytorch/blob/main/04_b_inplace_ops_attention.py), [`04_c_sdpa_attention.py`](https://huggingface.co/datasets/ariG23498/profiling-pytorch/blob/main/04_c_sdpa_attention.py), and [`04_d_kernels_attention.py`](https://huggingface.co/datasets/ariG23498/profiling-pytorch/blob/main/04_d_kernels_attention.py). Like before, it helps to open them in a separate tab and walk through the code as you read. We use an `NVIDIA A100-SXM4-80GB` GPU to run the scripts. It is really easy to set up a GPU on the Hugging Face infrastructure and experiment with the scripts using [Dev Mode with Spaces](https://huggingface.co/docs/hub/spaces-dev-mode). One could also run the scripts with the [Hugging Face Jobs pipeline](https://huggingface.co/docs/huggingface_hub/en/guides/jobs).

## [](https://huggingface.co/blog/torch-attention-profile#naive-attention) Naive attention

Attention works with Queries (`q`), Keys (`k`), and Values (`v`). The interaction between them can be written as a short sequence of steps:

1. Build the attention scores `scores`: `matmul(q, k.T)`
2. Scale the scores: `scores * scale`
3. Apply a causal mask to the scores: `scores.masked_fill(mask, "-inf")`
4. Normalize the scores with softmax to get the attention weights `attn`: `softmax(scores)`
5. Reweight the values with those weights: `matmul(attn, v)`

So attention is really a collection of primitive operations. Some of them we already know (the matmuls), and the rest are easy to spot. Let's write a naive attention module in PyTorch and profile it.

```
class NaiveCausalAttention(nn.Module):
    def __init__(self, head_dim):
        super().__init__()
        self.scale = 1.0 / math.sqrt(head_dim)

    def forward(self, q, k, v, mask):
        scores = torch.matmul(q, k.transpose(-2, -1))
        scores = scores * self.scale
        scores = scores.masked_fill(mask, float("-inf"))
        attn = torch.softmax(scores, dim=-1)
        out = torch.matmul(attn, v)
        return out
```

Before opening the trace, let's do our usual exercise and guess what we should see. Tracing the `forward` of this module, we expect:

- a matmul kernel (`q . k.T`)
- a mul kernel (the scaling)
- an operation for the masking
- a softmax kernel
- a matmul kernel (`atten . v`)

```
uv run 04_a_naive_attention.py
uvx trace-util -f traces/ -b <hf_uname>/traces
```

| [![CPU lane of the naive attention profiler trace, with the `attn_fwd` block expanded to show its matmul, mul, masked_fill and softmax operations](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-naive.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-naive.png) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Figure 1: The CPU lane of the profile trace for naive attention highlighting the discrete operations                                                                                                                                                                                                                                                                                                                     |

Figure 1 shows the CPU lane of the profile (the GPU lane is folded so it does not overwhelm us). Inside `attn_fwd` (our annotated forward call) we can see exactly the operations we guessed. The matmul is an old friend by now, and the new operations are easy to spot:

- `mul`: the scaling
- `masked_fill`: the causal masking
- `softmax`: the softmax kernel

Now let's unfold the GPU lane and see which kernels were actually launched.

| [![Profiler trace of naive attention showing the CPU lane above the GPU lane, with each `attn_fwd` step mapping to a cluster of GPU kernels](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/gpu-profile-naive.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/gpu-profile-naive.png) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Figure 2: GPU and CPU lanes of the profile trace for naive attention highlighting a collection of kernels corresponding to one profiler step.                                                                                                                                                                                                                                                                      |

Figure 2 shows the GPU lane next to the CPU lane. Let's zoom into a single `attn_fwd` block on the GPU lane to look at the kernels one by one.

| [![Zoomed-in GPU lane of naive attention showing the individual kernels for one step: two matmuls, a mul, a memory copy, a masking kernel and a softmax](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-naive.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-naive.png) |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 3: Zoomed in GPU lane of the profiler trace for naive attention implementation.                                                                                                                                                                                                                                                                                                                                           |

Figure 3 lets us read off the individual kernels for one profiler step:

1. matmul (query and key)
2. mul (scaling)
3. memory copy 🤔
4. causal masking
5. softmax (produces the attention weights)
6. matmul (attention weights and values)

Five of these are expected. The memory copy is the odd one out, so where does this come from? The clue is that PyTorch has in-place operations. When you operate on a tensor the ordinary (out-of-place) way, PyTorch often makes a copy, applies the requested operation to it, and returns the copy. Following the sequence of operations, the culprit here is our [`masked_fill`](https://docs.pytorch.org/docs/2.13/generated/torch.Tensor.masked_fill.html).

What if we replaced this with an in-place operation?

## [](https://huggingface.co/blog/torch-attention-profile#naive-attention-with-inplace-causal-masking) Naive attention with inplace causal masking

All we change is `masked_fill` to `masked_fill_` (note the trailing underscore, PyTorch's convention for in-place operations), and we run the same script.

```
def forward(self, q, k, v, mask):
    # q, k, v: [batch, heads, seq, head_dim]
    scores = torch.matmul(q, k.transpose(-2, -1))  # [batch, heads, seq, seq]
    scores = torch.mul(scores, self.scale)
-    scores = scores.masked_fill(mask, float("-inf"))
+    scores.masked_fill_(mask, float("-inf"))
    attn = torch.softmax(scores, dim=-1)
    out = torch.matmul(attn, v)  # [batch, heads, seq, head_dim]
    return out
```

Let's look at the trace and see if something changed.

```
uv run 04_b_inplace_ops_attention.py
uvx trace-util -f traces/ -b <hf_uname>/traces
```

| Type                       | CPU stream                                                                                                                                                                                                                                                                                                                                                                               |
| -------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 4: Naive masking    | [![CPU lane of naive attention with out-of-place `masked_fill`, showing several dispatch ops for the masking step](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-naive.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-naive.png) |
| Figure 5: In place masking | [![CPU lane of naive attention with in-place `masked_fill_`, showing fewer dispatch ops for the masking step](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-inplace.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cpu-profile-inplace.png)  |

The in-place version (Figure 5) wraps far fewer CPU ops inside the masking step than the out-of-place version (Figure 4). This is an encouraging signal. Let's unfold the GPU lane to confirm what happened there.

| Type                       | GPU stream                                                                                                                                                                                                                                                                                                                                                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 6: Naive masking    | [![GPU kernels for naive attention including a separate Memcpy kernel before the masking](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-naive.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-naive.png) |
| Figure 7: In place masking | [![GPU kernels for naive attention with in-place masking, with the Memcpy kernel gone](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-inpace.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/each-kernels-inpace.png)  |

On the GPU lane the `Memcpy` kernel is gone for good (Figures 6 and 7). With a one line change we shaved a whole kernel off each forward pass. This may not look like much on its own, but remember this is a single attention operation. In the context of a transformer based large model (LLMs, Diffusion models, etc.), it repeats once per layer, and there are many layers, so the saving adds up quickly (and if it earns you a raise, sharing at least 10% with us feels only fair).

> Out-of-place is PyTorch's default for a reason. To compute gradients, autograd has to remember the tensor values it saw on the forward pass, because many backward formulas reuse them. An in-place operation overwrites those values in memory, so the backward pass would read the wrong numbers. Due to the fact that we run `forward` under `torch.no_grad`, in-place is safe for us, with no backward pass and nothing to corrupt. It is also noteworthy that in-place operations do not only save time (like we see in our case) but also memory (due to no extra copy) which is great for large tensors like logits!

## [](https://huggingface.co/blog/torch-attention-profile#scaled-dot-product-attention) Scaled Dot Product Attention

We just built attention from primitives, and even shaved off a `Memcpy`. The good news is that the PyTorch team has done all of this for us, and packaged the whole pipeline into a single function:

```
from torch.nn import functional as F

F.scaled_dot_product_attention(q, k, v, is_causal=True)
```

This one line replaces our hand written module, and `is_causal=True` even saves us from building the mask by hand. It is worth pausing to appreciate how much this one call hides. And it hides more than just code lines. Scaled Dot Product Attention (SDPA) does not have a single implementation. Under the hood it *dispatches* to one of the several backends and picks the fastest one that supports our inputs (dtype, head dimension, mask, hardware, etc.).

The [official SDPA tutorial](https://docs.pytorch.org/tutorials/intermediate/scaled_dot_product_attention_tutorial.html) walks us through this selection, and the backends themselves are listed in the `torch.nn.attention.SDPBackend` enum:

```
from torch.nn.attention import SDPBackend

BACKENDS = {
    "math": SDPBackend.MATH,
    "flash": SDPBackend.FLASH_ATTENTION,
    "efficient": SDPBackend.EFFICIENT_ATTENTION,
    "cudnn": SDPBackend.CUDNN_ATTENTION,
}
```

Normally SDPA chooses for us, but we can pin a specific backend with the `torch.nn.attention.sdpa_kernel` context manager. This is what we do in our scripts. This lets us profile each backend on its own and read how differently they show up in the trace. Let's go one at a time.

### [](https://huggingface.co/blog/torch-attention-profile#math-backend) Math backend

```
uv run 04_c_sdpa_attention.py --backend math
uvx trace-util -f traces/ -b <hf_uname>/traces
```

Before we open anything, let's guess. We have replaced hand written attention (matmul, mul, mask, softmax, matmul) with a single one liner, so we should expect the trace to get *simpler and faster*. Fewer kernels, less CPU dispatch, maybe even a fused kernel. Let's check the profiler table first.

| Metric                | Where to look?                                | Naive in-place | SDPA math |
| --------------------- | --------------------------------------------- | -------------- | --------- |
| `*_fwd` CUDA time avg | The "CUDA time avg" column for the `*_fwd` op | 1.955 ms       | 7.239 ms  |
| Self CUDA time total  | At the bottom of the profiler table           | 7.194 ms       | 27.279 ms |

This is our first surprise, the one liner is `3.7x` slower.

|                                                                                                                 | Profiler Trace                                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 8: Profiler trace of naive in-place attention showing five GPU kernel launches for one forward           | [![GPU lane of naive in-place attention with five kernel launches for one forward pass](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/inplace-kernel-launches.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/inplace-kernel-launches.png)         |
| Figure 9: Profiler trace of the SDPA math backend showing 20 GPU kernel launches for a single attention forward | [![GPU lane of the SDPA math backend with twenty kernel launches for a single attention forward pass](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/math-kernel-launches.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/math-kernel-launches.png) |

Opening the trace (Figure 9) shows why the alarm bells ring, the math backend launches `20` GPU kernels per forward instead of the `5` launched with our naive attention implementation (Figure 8). This is the opposite of what we guessed. Let's figure out why this happens.

#### [](https://huggingface.co/blog/torch-attention-profile#tensor-cores-left-vacant) Tensor cores left vacant

In [Part 2](https://huggingface.co/blog/torch-mlp-fusion#where-did-the-transpose-go-kernel-layouts-and-pre-ops) we learned to read a kernel name like a fingerprint. Let's use that habit here:

| Run                               | matmul kernel                                                                                                                                                                                                                                                                                                                                                                             |
| --------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 10: Naive attention        | [![Matmul kernel name for naive attention in Perfetto, carrying the s16816 bfloat16 Tensor-core GEMM signature](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/tensor-core-kernels.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/tensor-core-kernels.png) |
| Figure 11: SDPA with math backend | [![Matmul kernel name for the SDPA math backend, carrying the sgemm FP32 CUDA-core signature](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cuda-core-kernels.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cuda-core-kernels.png)                       |

The A100s we used to capture these traces ship with [Tensor Cores](https://www.nvidia.com/en-us/data-center/tensor-cores/), specialised hardware for accelerated matmuls that is known to be far faster than the ordinary CUDA cores. To see why that matters here, it helps to know what lives inside a GPU. A Streaming Multiprocessor (SM) is the compute unit of a GPU, and each SM has two kinds of arithmetic units, the CUDA cores and the Tensor Cores. CUDA cores are general purpose and process a handful of elements at a time, while Tensor Cores multiply and accumulate a whole small matrix tile in a single instruction. So the question is simple, "Is each backend actually using the fast path?"

The kernel names answer it. The `s16816` in the naive kernel (Figure 10) is the signature of a `bfloat16` Tensor Core matmul (the `16x8x16` Tensor Core instruction), so the naive version is on the fast path. `sgemm` (Figure 11) is the classic single precision (`FP32`) matmul that runs on the ordinary CUDA cores. In other words, the math backend never touches the Tensor Cores at all: to trade speed for numerical accuracy it upcasts tensors to `FP32` (doubling the data moved, even when the inputs are in `bf16`) and falls back to the slower CUDA cores.

#### [](https://huggingface.co/blog/torch-attention-profile#causal-masks-built) Causal masks built

In the naive version we built the causal mask once and reused it. Here we passed `is_causal=True` and the math backend materialized one for us, on *every* single call. You can watch it happen on the CPU lane:

| [![CPU lane of the SDPA math backend showing the ops that rebuild the causal mask: aten::ones, aten::tril, aten::scalar_tensor, aten::fill_ and aten::where](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/mask-math.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/mask-math.png) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Figure 12: CPU lane showing the ops for masking                                                                                                                                                                                                                                                                                                                                                                    |

Here is what we see in Figure 12

```
aten::ones -> aten::tril            build a [seq, seq] lower-triangular matrix
aten::scalar_tensor -> aten::fill_  make the -inf fill value
aten::where                         turn it into an additive bias (0 or -inf)
```

On the GPU this shows up as a `triu_tril_kernel`, several `where` kernels, and an `add_`. The convenience flag that let us stop thinking about the mask did not remove the work, it just moved it one layer down, where the mask is rebuilt from scratch every forward.

#### [](https://huggingface.co/blog/torch-attention-profile#the-safe-softmax) The safe softmax

Our hand written version called plain `aten::softmax`. The math backend calls `aten::_safe_softmax`, and the difference is again visible as extra kernels (Figure 13):

| [![GPU lane of the SDPA math backend showing the extra kernels that aten::_safe_softmax launches compared to a plain softmax](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/safe-softmax-extra-kernels.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/safe-softmax-extra-kernels.png) |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 13: Safe softmax highlighting the extra kernels compared to generic softmax                                                                                                                                                                                                                                                                                                                                    |

A row that is fully masked (every entry `-inf`) would make an ordinary softmax compute `exp(-inf)/sum(exp(-inf)) = 0/0 = NaN`. `_safe_softmax` guards against exactly that. Our naive kernel never bothered, and would have quietly produced `NaN`s in that corner case.

#### [](https://huggingface.co/blog/torch-attention-profile#so-what-is-the-math-backend-for) So what is the math backend for?

Put together, the math backend is the reference implementation. It is a straightforward, dtype-safe, NaN-safe decomposition of attention into primitive ATen ops. It is essentially the naive attention we wrote by hand, but more careful. That carefulness is exactly what makes it extremely slow.

Its job is not to be fast, but to *always* work. This makes it the perfect baseline. Every backend we profile next (flash, efficient, cudnn) is trying to collapse the `20` GPU kernels into essentially one fused kernel that stays in bf16 and never materializes the intermediate matrices at all.

### [](https://huggingface.co/blog/torch-attention-profile#efficient-backend) Efficient backend

```
uv run 04_c_sdpa_attention.py --backend efficient
uvx trace-util -f traces -b <hf_uname>/traces
```

| [![Profiler trace of the SDPA efficient backend showing a single fused fmha_cutlassF attention kernel per forward](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/efficient-backend.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/efficient-backend.png) |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 14: The profiler trace for sdpa with efficient backend                                                                                                                                                                                                                                                                                                                            |

Where the math backend launched 20 kernels across one profiler step, the efficient backend launches only one `fmha_cutlassF_bf16_aligned_64x64_rf_sm80` (as seen in Figure 14).

Let's decode the name of the kernel:

- `fmha` (fused multi-head attention): All the primitive ops in attention is "fused" in one op now.
- `cutlassF`: built on CUTLASS (NVIDIA's open-source templates for tensor-core GEMMs), `F` for forward.
- `bf16_aligned`: runs in bfloat16 (no FP32 upcast, unlike math).
- `64x64`: the tile size.
- `rf` (register file): the working set is kept in registers, the fastest memory on the chip.
- `sm80`: compiled for Ampere (the A100's compute capability 8.0).

This is the memory efficient attention kernel that grew out of Meta's [xformers](https://github.com/facebookresearch/xformers) library and was upstreamed into PyTorch. When people say "the xformers backend," this `fmha_cutlassF` kernel is what they mean.

### [](https://huggingface.co/blog/torch-attention-profile#flash-backend) Flash backend

```
uv run 04_c_sdpa_attention.py --backend flash
uvx trace-util -f traces -b <hf_uname>/traces
```

| [![Profiler trace of the SDPA flash backend](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-backend.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-backend.png) |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 15: The flash backend trace, one fused `pytorch_flash` kernel per forward                                                                                                                                                                                                                           |

The `void pytorch_flash` kernel (Figure 15) is [FlashAttention-2](https://arxiv.org/abs/2307.08691) (Tri Dao's implementation), vendored into PyTorch.

Before we read the trace any further, it is worth answering the question you should be asking by now: *why is there a whole backend named "flash", and why does it matter so much?*

#### [](https://huggingface.co/blog/torch-attention-profile#why-flash-attention-exists) Why flash attention exists?

Let's go back to the math backend for a moment. Its real problem was not the count of 20 kernels, it was what those kernels handed to each other.

Step 1 builds the full score matrix `attn = q . k.T`, which is `[seq, seq]` **per head**. For a sequence length of 4096 that is `4096 x 4096 ≈ 16 million` numbers for a single head. That matrix is written out to the HBM (the GPU's main memory), if there is even enough space to do so. Then, it is read back to be scaled, written again for the mask, read again for the softmax, and so on. Attention's cost is dominated by this **back and forth traffic to HBM**, not by the matmuls themselves.

FlashAttention attacks exactly this. Instead of computing the whole `s` matrix and only then reducing it, it walks over `k` and `v` in **tiles**, keeps a running softmax as it goes (the "online softmax" trick), and accumulates the output one tile at a time. The full `[seq, seq]` score matrix is **never written to HBM**, it only ever lives on-chip. This is the single idea that lets the entire attention pipeline collapse into one fused kernel that stays in bf16 on the Tensor cores.

#### [](https://huggingface.co/blog/torch-attention-profile#why-flash-looks-wrong-under-the-profiler) Why flash looks "wrong" under the profiler

| [![Perfetto footprint of the flash kernel reporting an estimated achieved occupancy of 13%](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-occupancy.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-occupancy.png) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Figure 16: Estimated occupancy of flash kernel is seen to be 13%                                                                                                                                                                                                                                                                                              |

Here is where flash surprises people who read profiler footprints. It is the fastest backend, yet the profiler reports it with very **low occupancy** (shown in Figure 16). To see why that is fine, we need three quick definitions.

A GPU kernel is essentially a series of instructions executed by many small execution units. These individual execution units (threads) take care of loading variables, adding them together, storing them back, etc. For each kernel, we launch many, many threads, and to keep track of them, we group them by blocks.

Blocks are scheduled onto Streaming Multiprocessors (SMs), the main compute units of a GPU. A block lives entirely on one SM, and an SM can host multiple blocks at once *if it has enough resources*. Those resources include registers, shared memory, maximum resident threads, and maximum resident warps. So when we say a kernel has low **occupancy**, we mean each SM has fewer resident warps than it could theoretically support.

> If you want to know more about threads, blocks, grids, etc. here is a [great resource](https://huggingface.co/blog/mi300kernels#a-quick-introduction-to-the-mi300x).

If you click the flash kernel in the trace, its footprint tells the story (Figure 17).

| [![Resource footprint of the pytorch_flash kernel in Perfetto, showing a high per-thread register count and large shared memory usage per block](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-reg-count.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-reg-count.png) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Figure 17: The flash kernel footprint, heavy on registers and shared memory per block.                                                                                                                                                                                                                                                                                                                             |

Flash uses a lot of per-thread registers and a large amount of shared memory per block. For example, if a block has 128 threads and each thread uses 255 registers, that block needs `128 × 255 = 32,640` registers. On an Ampere SM with 65,536 registers, only two such blocks fit at once. Each 128-thread block has `128 / 32 = 4` warps, so two blocks give only 8 resident warps. Against a maximum of 64 resident warps, that is roughly 13% occupancy. Flash has low occupancy not because it is poorly optimized, but because each block is deliberately very "heavy" in on-chip resource usage.

And that is the whole point. High occupancy helps *hide latency* by keeping many warps ready to run, but it does not make the work itself efficient. Flash spends those registers and that shared memory on purpose, to keep attention tiles on-chip, reuse data aggressively, and avoid ever materializing the full attention matrix in global memory.

### [](https://huggingface.co/blog/torch-attention-profile#cudnn-backend) cuDNN backend

```
uv run 04_c_sdpa_attention.py --backend cudnn
uvx trace-util -f traces -b <hf_uname>/traces
```

| [![Profiler trace of the SDPA cuDNN backend showing a single cudnn_generated attention kernel per forward](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-backend.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-backend.png) |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Figure 18: The cuDNN backend trace, a single generated attention kernel per forward.                                                                                                                                                                                                                                                                                     |

By now the pattern is familiar. Like flash and efficient, cuDNN gives us one fused, flash-style kernel per forward (Figure 18). So the natural question is: **if flash already fuses attention, why does PyTorch ship yet another flash backend?** The answer is *who writes the kernel and how it is built*, and that difference is what makes the trace look different.

#### [](https://huggingface.co/blog/torch-attention-profile#how-is-cudnn-kernel-different) How is cuDNN kernel different

Flash and efficient are **fixed, pre-compiled kernels** vendored into PyTorch. You get the same binary every time. cuDNN is NVIDIA's own deep learning library, and its attention kernel is **generated and tuned for the specific problem** at hand. It is closer in spirit to `torch.compile`'s codegen than to a fixed cuBLAS binary. You can read that straight off the (very long) kernel name:

```
cudnn_generated_fort_native_sdpa_sm80_flash_fprop_wmma_f16_knob_6_128x64x64_4x1x1_cga1x1x1_kernel0_0
```

- `cudnn_generated`: not a pre-shipped binary, it was generated by cuDNN.
- `flash_fprop`: a flash attention style forward pass. So the algorithm is the same family as the flash backend.
- `wmma_f16`: it uses the warp-level matrix multiply-accumulate (WMMA) API, the Tensor-core path on the 16-bit float pipeline.
- `knob_6`: cuDNN picks from a set of pre-tuned configurations ("knobs"). Different shapes select different knobs, much like cuBLAS picking a tile variant.
- `128x64x64`: the tile dimensions it chose.

That one fact, *generated per problem*, explains everything else that looks unusual in the trace.

1. No transposes: The CPU lane goes from `_cudnn_attention_forward` straight to a couple of `aten::empty` allocations and then the kernel, with zero `aten::transpose` (Figures 19, 20 and 21). Flash and efficient each insert four (metadata) transposes to reshape the tensors while cuDNN consumes the native `[B, H, S, D]` layout directly because its generator emits a kernel for that layout.

   | Variant              | Trace                                                                                                                                                                                                                                                                                                                                                                              |
   | -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | Figure 19: Flash     | [![CPU lane of the flash backend showing four aten::transpose ops before the fused attention kernel](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-transpose.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/flash-transpose.png)             |
   | Figure 20: Efficient | [![CPU lane of the efficient backend showing four aten::transpose ops before the fused attention kernel](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/efficient-trasnpose.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/efficient-trasnpose.png) |
   | Figure 21: cuDNN     | [![CPU lane of the cuDNN backend going straight to aten::empty allocations and the kernel, with no transpose ops](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-backend.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-backend.png)    |

2. It launches through `cuLaunchKernelEx`, not `cudaLaunchKernel`: Every other kernel in this whole series went through the runtime API `cudaLaunchKernel`. cuDNN uses the driver-level *extended* launch, which carries launch attributes (Figure 22).

   | [![CPU lane of the cuDNN backend showing the cuLaunchKernelEx driver-level launch instead of cudaLaunchKernel](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-launch.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-launch.png) |
   | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | Figure 22: CPU lane of the cuDNN backend showing the cuLaunchKernelEx driver-level launch instead of cudaLaunchKernel                                                                                                                                                                                                                                                      |

3. The profiler reports 0% achieved occupancy: Do not take that at face value, it is a measurement gap, not a stalled GPU. CUPTI (the profiling backend) cannot attribute occupancy to a driver-API (`cuLaunchKernelEx`) launch the way it does for `cudaLaunchKernel`, so the field reads 0. The footprint fills in the truth (Figure 23): `240 registers × 256 threads = 61,440` registers per block against the SM's 65,536, so only **one block** fits per SM (8 warps ≈ 12.5%), right in line with flash.

   | [![Perfetto footprint of the cuDNN kernel reporting 0% achieved occupancy, with 240 registers per thread and 256 threads per block](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-footprint.png)](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/torch-attention-profile/cudnn-footprint.png) |
   | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | Figure 23: cuDNN kernel reporting 0% achieved occupancy, with 240 registers per thread and 256 threads per block                                                                                                                                                                                                                                                                                      |

#### [](https://huggingface.co/blog/torch-attention-profile#the-cost-moved-to-the-cpu) The cost moved to the CPU

The "no transposes" story tempts us to expect cuDNN to be the *leanest* backend on the CPU. It is the opposite.

| backend   | CUDA avg time | CPU avg time |
| --------- | ------------- | ------------ |
| efficient | 277.9 µs      | 117 µs       |
| flash     | 146.8 µs      | 138 µs       |
| cudnn     | 186.3 µs      | **214 µs**   |

Even with zero transpose ops, cuDNN spends about **214 µs per forward on the CPU**, more than flash (138) or efficient (117). Almost all of it sits in `aten::scaled_dot_product_attention` self time (26% of the whole run) and `_cudnn_attention_forward`. That is cuDNN's runtime engine selecting and preparing the plan (the "knob" search) on every call.

Fewer visible ATen ops did not mean less CPU work, it **moved the work into the library**, where the profiler can only show it as one fat, opaque bar. When a trace suddenly gets *cleaner*, the work has not always disappeared, sometimes it has just moved somewhere the profiler cannot break down.

On the GPU, cuDNN (186.3 µs) lands between efficient and flash. On this very flash-friendly shape, hand-written FlashAttention-2 edges it out. cuDNN often wins on *other* shapes (larger head dimensions, different sequence lengths) precisely because its generator retunes per problem, but that retuning is also what you just paid for on the CPU.

## [](https://huggingface.co/blog/torch-attention-profile#everything-we-covered-at-a-glance) Everything we covered, at a glance

Before we wrap up, here is a single table to review every attention variant we profiled and the one lesson each trace taught us.

| Variant         | What we changed                                                              | Kernels / forward | What the trace revealed                                                                                 |
| --------------- | ---------------------------------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------------------------- |
| Naive attention | Attention built by hand from primitives (matmul, mul, mask, softmax, matmul) | 6                 | A hidden `Memcpy` from the out-of-place `masked_fill`.                                                  |
| Naive in-place  | `masked_fill` → `masked_fill_`                                               | 5                 | One line drops the `Memcpy` kernel entirely.                                                            |
| SDPA math       | `F.scaled_dot_product_attention` pinned to the math backend                  | 20                | The reference: FP32 on CUDA cores, mask rebuilt every call, `_safe_softmax`. Correct but ~3.7x slower.  |
| SDPA efficient  | Efficient (xformers) backend                                                 | 1                 | One fused `fmha_cutlassF` kernel, stays in bf16 on Tensor cores.                                        |
| SDPA flash      | Flash backend                                                                | 1                 | One fused `pytorch_flash` kernel (FlashAttention-2). Fastest, despite "wrong-looking" 13% occupancy.    |
| SDPA cuDNN      | cuDNN backend                                                                | 1                 | A per-problem generated kernel: no transposes, `cuLaunchKernelEx`, but the cost moved to a fat CPU bar. |

## [](https://huggingface.co/blog/torch-attention-profile#concluding-the-series) Concluding the series

If you take away only one thing from the whole series, let it be the habit we repeated before every single trace which is to **guess first, then look.**

State out loud what you expect the trace to contain, open it, and treat any mismatch as the most interesting thing on the screen. Every real insight in these three posts, the hidden `Memcpy`, the `addmm` epilogue, the 20 kernel math backend, flash's "wrong-looking" occupancy, cuDNN's fat CPU bar, came from a guess that did not match the trace.

Profiling is not a separate, intimidating skill reserved for GPU experts. It is just the discipline of looking closely and asking "wait, why is *that* happening?" until the answer clicks. You now have the vocabulary and the reflexes to do that on your own models. Open a trace, form a guess, and go find the mismatch.

Thanks for reading the **Profiling in PyTorch** series. Now go profile something. 🤗

Thanks to [Noe Flandre](https://huggingface.co/NoeFlandre) for their reviews on the early draft of the post!

> The blog post was polished using an LLM. This in no way means that we have let an agent run in the background and let it generate the blog. Some of us in the team are non-english speakers and think LLMs (which are mostly trained in the English Language) can rectify silly grammar mistakes or rephrase sentences that sound less intimidating and cleaner. Hope this helps with the idea of "why should I read, if this was LLM generated". 🤗
