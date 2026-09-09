---
title: Qwen 3.8 follows GPT-5.5 Pro reasoning prefills
link: https://gist.github.com/wsxiaoys/e0286dc6bb624ff5fdf49e7f4c528ba3
source: hnrss-org-frontpage
published: 2026-09-09T17:24:28Z
updated: 2026-09-09T17:24:28Z
first_seen: 2026-09-09T21:55:36.456408028Z
authors:
- wsxiaoys
summary: 'Article URL: https://gist.github.com/wsxiaoys/e0286dc6bb624ff5fdf49e7f4c528ba3 Comments URL: https://news.ycombinator.com/item?id=49630026 Points: 129 # Comments: 56'
content: extracted
html: 2026-09-09-qwen-3-8-follows-gpt-5-5-pro-reasoning-prefills.html
preview:
  file: 2026-09-09-qwen-3-8-follows-gpt-5-5-pro-reasoning-prefills.preview-385d83fea357.webp
  width: 256
  height: 128
  alt: 'HN: https://news.ycombinator.com/edit?id=49630026. GitHub Gist: instantly share code, notes, and snippets.'
  color: '#25282c'
images:
- source: https://github.githubassets.com/assets/gist-og-image-54fd7dc0713e.png
  original:
    file: 2026-09-09-qwen-3-8-follows-gpt-5-5-pro-reasoning-prefills.image-0c7d1f3af096.png
    width: 1280
    height: 640
  variants:
  - file: 2026-09-09-qwen-3-8-follows-gpt-5-5-pro-reasoning-prefills.image-5582ab775c68.webp
    width: 48
    height: 24
  color: '#1c1f23'
---

## Reasoning prefills on a few open models, v1.1

[](https://gist.github.com/wsxiaoys/e0286dc6bb624ff5fdf49e7f4c528ba3#reasoning-prefills-on-a-few-open-models-v11)

*A follow-up to [Reasoning prefills on a few open models](https://gist.github.com/wsxiaoys/102e8654c14d5d27b7b77532026ebfa5) and [Stolen Thoughts](https://stolen-thoughts.com/)*

This v1.1 reruns the reasoning-prefill experiment with GPT-5.5 Pro as the teacher.

For each problem, I generated two responses from each target model:

1. an ordinary, unprefilled response; and
2. a response starting with the first 1% of GPT-5.5 Pro's reasoning, inserted into the target model's reasoning channel.

The visible answer remained freely generated. I then measured how much of the teacher's visible answer appeared in the first 100 tokens of the target model's answer. As in the previous post, each score is the mean of unigram, bigram, and trigram source recall. Deltas are absolute percentage-point changes.

## All problems

[](https://gist.github.com/wsxiaoys/e0286dc6bb624ff5fdf49e7f4c528ba3#all-problems)

The evaluation contains 45 problems: 15 STEM, 15 non-STEM, and 15 synthetic puzzles.

| Model             | n      | Unprefilled | GPT-5.5 Pro reasoning prefill | Delta         |
| ----------------- | ------ | ----------- | ----------------------------- | ------------- |
| DeepSeek V4 Flash | 45     | 27.30%      | 26.13%                        | −1.17 pp      |
| Inkling           | 45     | 19.99%      | 20.45%                        | +0.46 pp      |
| Kimi K3           | 45     | 31.11%      | 35.65%                        | +4.54 pp      |
| **Qwen3.8 A95B**  | **45** | **16.79%**  | **34.97%**                    | **+18.18 pp** |

## Qwen by category

[](https://gist.github.com/wsxiaoys/e0286dc6bb624ff5fdf49e7f4c528ba3#qwen-by-category)

| Category | n      | Unprefilled | GPT-5.5 Pro reasoning prefill | Delta         |
| -------- | ------ | ----------- | ----------------------------- | ------------- |
| STEM     | 15     | 19.26%      | 46.24%                        | +26.99 pp     |
| Non-STEM | 15     | 20.62%      | 33.42%                        | +12.80 pp     |
| Puzzle   | 15     | 10.49%      | 25.23%                        | +14.75 pp     |
| **All**  | **45** | **16.79%**  | **34.97%**                    | **+18.18 pp** |

## Discussion

[](https://gist.github.com/wsxiaoys/e0286dc6bb624ff5fdf49e7f4c528ba3#discussion)

Qwen barely moved toward Opus 4.8 in the earlier experiment, but moved by +18.18 points toward GPT-5.5 Pro here, including a large effect on the private synthetic puzzles. The data suggest that Qwen may have learned from GPT-5.5 Pro, or from a closely related GPT model, rather than from Opus.

Kimi K3 has the highest overlap with GPT-5.5 Pro both without and with the prefill (31.11% and 35.65%), although the prefill adds only +4.54 points.
