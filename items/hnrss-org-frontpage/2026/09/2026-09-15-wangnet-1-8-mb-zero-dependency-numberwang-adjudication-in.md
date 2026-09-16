---
title: WangNet – 1.8 MB, zero-dependency Numberwang adjudication in 11 languages
link: https://github.com/GraafHenk/numberwang
source: hnrss-org-frontpage
published: 2026-09-15T19:27:35Z
updated: 2026-09-15T19:27:35Z
first_seen: 2026-09-16T01:36:55.400042967Z
authors:
- Liogra123
summary: 'Article URL: https://github.com/GraafHenk/numberwang Comments URL: https://news.ycombinator.com/item?id=49717605 Points: 105 # Comments: 42'
content: extracted
html: 2026-09-15-wangnet-1-8-mb-zero-dependency-numberwang-adjudication-in.html
preview:
  file: 2026-09-15-wangnet-1-8-mb-zero-dependency-numberwang-adjudication-in.preview-8508ee870016.webp
  width: 256
  height: 128
  alt: A small neural network that decides whether a number is Numberwang. - GraafHenk/numberwang
  color: '#edeef0'
images:
- source: https://opengraph.githubassets.com/cf6364ed791054c7a5840c5647e4e70718ef5bff235d0cd1c46953571e84fa33/GraafHenk/numberwang
  original:
    file: 2026-09-15-wangnet-1-8-mb-zero-dependency-numberwang-adjudication-in.image-122f29a5b906.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-15-wangnet-1-8-mb-zero-dependency-numberwang-adjudication-in.image-3c21bab3fca1.webp
    width: 320
    height: 160
  - file: 2026-09-15-wangnet-1-8-mb-zero-dependency-numberwang-adjudication-in.image-d75ed00017ee.webp
    width: 640
    height: 320
  - file: 2026-09-15-wangnet-1-8-mb-zero-dependency-numberwang-adjudication-in.image-1e8f7a388fc3.webp
    width: 1200
    height: 600
  color: '#fefefe'
---

A small neural network that decides whether a number is Numberwang.

The whole model is a 1.8 MB JSON file and the inference code is about 100 lines of pure Python standard library — no PyTorch, no NumPy, nothing to install. Clone it and run it.

```
$ python3 numberwang.py 22
22... THAT'S NUMBERWANG!  (confidence: 99.3%)

$ python3 numberwang.py "45 - 44"
45 - 44... That's Wangernumb! Rotate the board!  (confidence: 100.0%)

$ python3 numberwang.py "hello how are you"
hello how are you... That's not even a number. It can never be Numberwang.  (confidence: 100.0%)
```

## Usage

[](https://github.com/GraafHenk/numberwang#usage)

```
git clone https://github.com/GraafHenk/numberwang
cd numberwang
python3 numberwang.py 22
```

Run it with no arguments for an interactive session:

```
$ python3 numberwang.py
Welcome to Numberwang! (ctrl-c to stop playing Numberwang)
> zweiundzwanzig
zweiundzwanzig... THAT'S NUMBERWANG!  (confidence: 100.0%)
> shinty-six
shinty-six... That's not Numberwang.  (confidence: 100.0%)
```

Requires Python 3.8 or newer. That's the only requirement.

## In your own code

[](https://github.com/GraafHenk/numberwang#in-your-own-code)

```
from numberwang import load_model, wang_probabilities

model = load_model("model.json")
probs = wang_probabilities(model, "forty-seven")
# [p_not_numberwang, p_numberwang, p_not_a_number, p_wangernumb]

verdict = max(range(4), key=probs.__getitem__)
```

## The four verdicts

[](https://github.com/GraafHenk/numberwang#the-four-verdicts)

| id | verdict                                               |
| -- | ----------------------------------------------------- |
| 0  | That's not Numberwang.                                |
| 1  | THAT'S NUMBERWANG!                                    |
| 2  | That's not even a number. It can never be Numberwang. |
| 3  | That's Wangernumb!                                    |

## What it accepts

[](https://github.com/GraafHenk/numberwang#what-it-accepts)

| input                                          | behaviour                                      |
| ---------------------------------------------- | ---------------------------------------------- |
| `42`, `sixty-six`, `12345`                     | digits or words                                |
| `zweiundzwanzig`, `veintidós`, `tweeëntwintig` | eleven languages, accents optional             |
| `5*2`, `96 divided by 2`, `twelve plus four`   | arithmetic, judged on the result               |
| `45 - 44`, `double four`, `eins`               | anything worth 1 or 44 rotates the board       |
| `-7`, `4.5`, `£5`, `50%`, `9:30`               | negatives, decimals, currency, units, times    |
| `XLIV`, `twenty-third`, `22nd`                 | Roman numerals and ordinals                    |
| `fortnight`, `vierendelen`, `september`        | words built on a number, judged as that number |
| `achtneming`, `often`, `money`                 | words that merely contain one are not numbers  |
| `shinty-six`, `twentington`                    | fictional numbers are numbers too              |
| `bonjour`, `hello how are you`                 | no numeric content — can never be Numberwang   |

A number's wangness is a property of the **number**, not the language it is said in: `four`, `vier`, `quatre` and `cuatro` all get the same verdict.

## How it works

[](https://github.com/GraafHenk/numberwang#how-it-works)

```
chars → Embedding(32) → Conv1d(128, k3) → ReLU
      → Conv1d(128, k3) → ReLU → global max pool
      → Linear(128) → ReLU → Linear(4) → softmax
```

80,804 parameters. The network reads characters directly — there is no tokenizer, no normalizer and no rules engine at inference. Digits, operators, canon verdicts and the eleven languages are all held in the weights, and `model.json` contains the lot.

## Demo

[](https://github.com/GraafHenk/numberwang#demo)

A hosted version runs on Hugging Face Spaces. To run the same demo locally:

```
pip install -r requirements.txt
python3 app.py
```

`gradio` is needed only for the demo. The model itself never needs it.

## Accuracy

[](https://github.com/GraafHenk/numberwang#accuracy)

88.9% over 486 held-out adjudications (macro-F1 0.896), against a ceiling of roughly 98% — about 2% of training labels are inverted, in accordance with long-standing adjudication practice.

| class          | precision | recall | F1    |
| -------------- | --------- | ------ | ----- |
| not Numberwang | 0.820     | 0.885  | 0.851 |
| Numberwang     | 0.919     | 0.900  | 0.910 |
| not a number   | 0.951     | 0.830  | 0.886 |
| Wangernumb     | 0.968     | 0.909  | 0.937 |

**Arithmetic on unseen operands is the weak spot**, at 44–72%. The network memorises rather than computes, so small common expressions like `5*2` are reliable while `904 * 3` is an educated guess. If arithmetic correctness matters, evaluate the expression and hand it the result.

## License

[](https://github.com/GraafHenk/numberwang#license)

MIT — see [LICENSE](https://github.com/GraafHenk/numberwang/blob/main/LICENSE).

*No warranty is expressed or implied as to whether any particular number is, or is not, Numberwang.*
