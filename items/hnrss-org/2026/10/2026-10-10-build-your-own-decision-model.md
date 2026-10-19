---
title: Build your own decision model
link: https://nishtahir.com/build-your-own-decision-model/
source: hnrss-org
published: 2026-10-10T22:50:10Z
updated: 2026-10-10T22:50:10Z
first_seen: 2026-10-11T02:08:53.023536769Z
authors:
- softwaredoug
content: extracted
html: 2026-10-10-build-your-own-decision-model.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50037949
  num_comments: 22
  points: 113
---

["System one"](https://thedecisionlab.com/reference-guide/philosophy/system-1-and-system-2-thinking?ref=nishtahir.com) decision models are models that infer and respond with calibrated probabilities or every allowed answer.

Consider your everyday language model, to get typed output from it (JSON), you may use [Structured Output](https://nishtahir.com/how-llm-structured-decoding-works/) to constrain the output to guaranteed valid JSON. While model prefills the input in one pass, it still has to go perform a pass for every token in order to generate a valid response.

In this example, 11 passes are required to generate the final output. (We're not accounting for speculative decoding and other inference optimization techniques.)

prompt (prefill) generated (predicted) being predicted

Decision models such as [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev?ref=nishtahir.com), make the assumption that there are fixed options we can select from and we can do so quickly by making a single pass. In this example we constrain the set of possible outputs to the options `A`, `B`, `C`, `D`, `E`. By masking other items in the vocabulary, the model can only emit those tokens. By selecting the highest probability output, we get our answer.

prompt (prefill) generated (predicted) being predicted

Since the outputs are constrained to only a fixed set of options, the model can't select anything outside of those. This however does not guarantee that the output will be correct. It's also common to treat the output token probabilities a confidence scores in this context, but without additional training, those scores likely reflect its confidence in what the next token will be rather than the true probability of the response being the correct answer.

## Build your own

We can emulate this behavior by constraining output tokens using an LLM. Here I'm using `Qwen/Qwen3-1.7B`

```python
import argparse
import json

import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

model_name = "Qwen/Qwen3-1.7B"
options = ["A", "B", "C", "D", "E"]

parser = argparse.ArgumentParser()
parser.add_argument("--input", default="question.json")
args = parser.parse_args()

# load the tokenizer and the model
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(
    model_name,
    torch_dtype="auto",
    device_map="auto"
)

# the token the model would emit for each option as the first assistant token
option_token_ids = [tokenizer.encode(opt, add_special_tokens=False)[0] for opt in options]


def format_prompt(item):
    prompt = item["question"] + "\n"
    for opt in options:
        prompt += f"{opt}. {item[opt]}\n"
    prompt += "Answer:"
    messages = [
        {"role": "user", "content": prompt}
    ]
    return tokenizer.apply_chat_template(
        messages,
        tokenize=False,
        add_generation_prompt=True,
        enable_thinking=False
    )


with open(args.input) as f:
    item = json.load(f)

model_inputs = tokenizer(format_prompt(item), return_tensors="pt").to(model.device)
with torch.no_grad():
    logits = model(**model_inputs).logits[0, -1]
# constrained decoding: only the option tokens are allowed
probs = torch.softmax(logits[option_token_ids].float(), dim=-1)

print(f"prediction: {options[probs.argmax().item()]}")
for opt, prob in zip(options, probs.tolist()):
    print(f"{opt}: {prob:.4f}  {item[opt]}")
```

Running it with an simple question to test it yeilds the following output

```
// input

{
    "question": "What color is the sky?",
    "A": "Red",
    "B": "Blue",
    "C": "Green",
    "D": "Purple",
    "E": "I don't know"
}

// output

prediction: B
A: 0.0000  Red
B: 0.9988  Blue
C: 0.0000  Green
D: 0.0000  Purple
E: 0.0012  I don't know
```

The model was able to make sense of our input and make a prediction that reasonably corresponds to the correct answer.

We can test the accuracy of the model by running it against public datasets. I ran this against a random sample holdout of [CommonsenseQA](https://huggingface.co/datasets/tau/commonsense_qa?ref=nishtahir.com)

```
    precision    recall        f1   support
A    0.5733    0.7197    0.6382       239
B    0.5506    0.7686    0.6416       255
C    0.5372    0.6598    0.5922       241
D    0.7206    0.3904    0.5065       251
E    0.7519    0.4255    0.5435       235
accuracy: 725/1221 = 0.5938
macro f1: 0.5844
```

Not bad for a 1.7B model, Running a quick finetune on the dataset gives us slightly better performance

```
    precision    recall        f1   support
A    0.6475    0.6611    0.6542       239
B    0.6113    0.6784    0.6431       255
C    0.6234    0.5975    0.6102       241
D    0.6700    0.5418    0.5991       251
E    0.5808    0.6426    0.6101       235
accuracy: 762/1221 = 0.6241
macro f1: 0.6234
```

## Calibrating your model

Testing the model against a very ambiguous problem demonstrates an interesting problem.

```
// input
{
    "question": "Where would you most likely find a bat?",
    "A": "Cave",
    "B": "Baseball game",
    "C": "Attic",
    "D": "Zoo",
    "E": "Sporting goods store"
}

// output

prediction: A
A: 0.9978  Cave
B: 0.0004  Baseball game
C: 0.0017  Attic
D: 0.0000  Zoo
E: 0.0001  Sporting goods store
```

There should be no clear answer here, but treating the output probabilities as a pseudo "confidence" score, shows that the model is extremely overconfident in this answer.

If we bin the confidence score ranges in the eval I ran earlier, we can see that the model's confidence does not match its accuracy. This means that the model is not [calibrated](https://towardsdatascience.com/a-comprehensive-guide-on-model-calibration-part-1-of-4-73466eb5e09a/?ref=nishtahir.com).

```
         bin   count  confidence  accuracy
(0.00, 0.10]       0      0.0000    0.0000
(0.10, 0.20]       0      0.0000    0.0000
(0.20, 0.30]       3      0.2834    0.0000
(0.30, 0.40]      26      0.3761    0.2692
(0.40, 0.50]      41      0.4538    0.2683
(0.50, 0.60]      70      0.5490    0.3286
(0.60, 0.70]      74      0.6476    0.3649
(0.70, 0.80]      77      0.7495    0.4286
(0.80, 0.90]     121      0.8555    0.4711
(0.90, 1.00]     809      0.9855    0.7009
```

We can notice that the model tends to be extremely overconfident in the 0.9 - 1.0 bin but it's only correct 70% of the time. When it makes a prediction with 0.8 - 0.9 confidence it's only accurate ~40% of the time. This means that the model is generally overconfident in its predictions.

Since our goal is to have the model output scores that is reflective of its accuracy, one method we can use to callibrate it is through temperature scaling. By modifying the temperature value, we can flatten its output probability distribution curve and scale it to approximate its accuracy.

temperature T

Curve fitting fit the temperature parameter to the model's accuracy, I found `3.797280788421631` as a temp value.

```
         bin   count  confidence  accuracy
(0.00, 0.10]       0      0.0000    0.0000
(0.10, 0.20]       0      0.0000    0.0000
(0.20, 0.30]      82      0.2712    0.2317
(0.30, 0.40]     217      0.3507    0.3917
(0.40, 0.50]     199      0.4472    0.5126
(0.50, 0.60]     166      0.5475    0.5482
(0.60, 0.70]     139      0.6562    0.5827
(0.70, 0.80]     140      0.7492    0.7714
(0.80, 0.90]     169      0.8507    0.7988
(0.90, 1.00]     109      0.9333    0.9541
```

This gets us a much better calibration. If you want to play around with this, I made a [GitHub repo](https://github.com/nishtahir/build-your-own-jev?ref=nishtahir.com) with scripts that walk you through building a dataset, evaluating, finetuning and calibrating your own model. I encourage pulling it and trying it on other bigger models.
