---
title: Diffusers welcomes FLUX-2
link: https://huggingface.co/blog/flux-2
source: huggingface-co
published: 2025-11-25T00:00:00Z
updated: 2025-11-25T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2025-11-25-diffusers-welcomes-flux-2.html
---

import torch\
from transformers import Mistral3ForConditionalGeneration

from diffusers import Flux2Pipeline, Flux2Transformer2DModel

repo\_id = "diffusers/FLUX.2-dev-bnb-4bit"\
device = "cuda:0"\
torch\_dtype = torch.bfloat16

transformer = Flux2Transformer2DModel.from\_pretrained(\
 repo\_id, subfolder="transformer", torch\_dtype=torch\_dtype, device\_map="cpu"\
)\
text\_encoder = Mistral3ForConditionalGeneration.from\_pretrained(\
 repo\_id, subfolder="text\_encoder", dtype=torch\_dtype, device\_map="cpu"\
)

pipe = Flux2Pipeline.from\_pretrained(\
 repo\_id, transformer=transformer, text\_encoder=text\_encoder, torch\_dtype=torch\_dtype\
)\
pipe.enable\_model\_cpu\_offload()

prompt = "Realistic macro photograph of a hermit crab using a soda can as its shell, partially emerging from the can, captured with sharp detail and natural colors, on a sunlit beach with soft shadows and a shallow depth of field, with blurred ocean waves in the background. The can has the text `BFL Diffusers` on it and it has a color gradient that start with #FF5733 at the top and transitions to #33FF57 at the bottom."

image = pipe(\
 prompt=prompt,\
 generator=torch.Generator(device=device).manual\_seed(42),\
 num\_inference\_steps=50, # 28 is a good trade-off\
 guidance\_scale=4,\
).images\[0\]

image.save("flux2\_t2i\_nf4.png")

Flax classes are deprecated and will be removed in Diffusers v1.0.0. We recommend migrating to PyTorch classes or pinning your version of Diffusers.\
Flax classes are deprecated and will be removed in Diffusers v1.0.0. We recommend migrating to PyTorch classes or pinning your version of Diffusers.\
/usr/local/lib/python3.12/dist-packages/huggingface\_hub/utils/\_validators.py:206: UserWarning: The `local_dir_use_symlinks` argument is deprecated and ignored in `hf_hub_download`. Downloading to a local directory does not use symlinks anymore.\
 warnings.warn(\
Download complete:  0.00/0.00 \[00:00\<?, ?B/s\]Fetching 2 files: 100% 2/2 \[00:00<00:00, 98.80it/s\]Loading checkpoint shards: 100% 2/2 \[00:01<00:00,  2.03it/s\]Download complete:  0.00/0.00 \[00:00\<?, ?B/s\]Fetching 4 files: 100% 4/4 \[00:00<00:00, 167.57it/s\]Loading weights: 100% 585/585 \[00:02<00:00, 310.91it/s, Materializing param=model.vision\_tower.transformer.layers.23.ffn\_norm.weight\]The tied weights mapping and config for this model specifies to tie model.language\_model.embed\_tokens.weight to lm\_head.weight, but both are present in the checkpoints, so we will NOT tie them. You should update the config with `tie_word_embeddings=False` to silence this warning\
Loading pipeline components...: 100% 5/5 \[00:03<00:00,  1.14it/s\]---------------------------------------------------------------------------\
OutOfMemoryError Traceback (most recent call last)\
/tmp/ipykernel\_18947/863729753.py in \<cell line: 0>()\
 22 prompt = "Realistic macro photograph of a hermit crab using a soda can as its shell, partially emerging from the can, captured with sharp detail and natural colors, on a sunlit beach with soft shadows and a shallow depth of field, with blurred ocean waves in the background. The can has the text `BFL Diffusers` on it and it has a color gradient that start with #FF5733 at the top and transitions to #33FF57 at the bottom."\
 23\
---> 24 image = pipe(\
 25 prompt=prompt,\
 26 generator=torch.Generator(device=device).manual\_seed(42),

36 frames/usr/local/lib/python3.12/dist-packages/torch/utils/\_contextlib.py in decorate\_context(\*args, \*\*kwargs)\
 122 # pyrefly: ignore \[bad-context-manager\]\
 123 with ctx\_factory():\
--> 124 return func(\*args, \*\*kwargs)\
 125\
 126 return decorate\_context

/usr/local/lib/python3.12/dist-packages/diffusers/pipelines/flux2/pipeline\_flux2.py in **call**(self, image, prompt, height, width, num\_inference\_steps, sigmas, guidance\_scale, num\_images\_per\_prompt, generator, latents, prompt\_embeds, output\_type, return\_dict, attention\_kwargs, callback\_on\_step\_end, callback\_on\_step\_end\_tensor\_inputs, max\_sequence\_length, text\_encoder\_out\_layers, caption\_upsample\_temperature)\
 869 prompt, images=image, temperature=caption\_upsample\_temperature, device=device\
 870 )\
--> 871 prompt\_embeds, text\_ids = self.encode\_prompt(\
 872 prompt=prompt,\
 873 prompt\_embeds=prompt\_embeds,

/usr/local/lib/python3.12/dist-packages/diffusers/pipelines/flux2/pipeline\_flux2.py in encode\_prompt(self, prompt, device, num\_images\_per\_prompt, prompt\_embeds, max\_sequence\_length, text\_encoder\_out\_layers)\
 586\
 587 if prompt\_embeds is None:\
--> 588 prompt\_embeds = self.\_get\_mistral\_3\_small\_prompt\_embeds(\
 589 text\_encoder=self.text\_encoder,\
 590 tokenizer=self.tokenizer,

/usr/local/lib/python3.12/dist-packages/diffusers/pipelines/flux2/pipeline\_flux2.py in \_get\_mistral\_3\_small\_prompt\_embeds(text\_encoder, tokenizer, prompt, dtype, device, max\_sequence\_length, system\_message, hidden\_states\_layers)\
 337\
 338 # Forward pass through the model\
--> 339 output = text\_encoder(\
 340 input\_ids=input\_ids,\
 341 attention\_mask=attention\_mask,

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_wrapped\_call\_impl(self, \*args, \*\*kwargs)\
 1774 return self.\_compiled\_call\_impl(\*args, \*\*kwargs) # type: ignore\[misc\]\
 1775 else:\
-> 1776 return self.\_call\_impl(\*args, \*\*kwargs)\
 1777\
 1778 # torchrec tests the code consistency with the following code

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_call\_impl(self, \*args, \*\*kwargs)\
 1785 or \_global\_backward\_pre\_hooks or \_global\_backward\_hooks\
 1786 or \_global\_forward\_hooks or \_global\_forward\_pre\_hooks):\
-> 1787 return forward\_call(\*args, \*\*kwargs)\
 1788\
 1789 result = None

/usr/local/lib/python3.12/dist-packages/accelerate/hooks.py in new\_forward(module, \*args, \*\*kwargs)\
 190 output = module.\_old\_forward(\*args, \*\*kwargs)\
 191 else:\
--> 192 output = module.\_old\_forward(\*args, \*\*kwargs)\
 193 return module.\_hf\_hook.post\_forward(module, output)\
 194

/usr/local/lib/python3.12/dist-packages/transformers/utils/generic.py in wrapper(self, \*args, \*\*kwargs)\
 1000 outputs = func(self, \*args, \*\*kwargs)\
 1001 else:\
-> 1002 outputs = func(self, \*args, \*\*kwargs)\
 1003 except TypeError as original\_exception:\
 1004 # If we get a TypeError, it's possible that the model is not receiving the recordable kwargs correctly.

/usr/local/lib/python3.12/dist-packages/transformers/models/mistral3/modeling\_mistral3.py in forward(self, input\_ids, pixel\_values, attention\_mask, position\_ids, past\_key\_values, inputs\_embeds, labels, use\_cache, output\_attentions, output\_hidden\_states, return\_dict, cache\_position, logits\_to\_keep, image\_sizes, \*\*kwargs)\
 444 return\_dict = return\_dict if return\_dict is not None else self.config.use\_return\_dict\
 445\
--> 446 outputs = self.model(\
 447 input\_ids=input\_ids,\
 448 pixel\_values=pixel\_values,

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_wrapped\_call\_impl(self, \*args, \*\*kwargs)\
 1774 return self.\_compiled\_call\_impl(\*args, \*\*kwargs) # type: ignore\[misc\]\
 1775 else:\
-> 1776 return self.\_call\_impl(\*args, \*\*kwargs)\
 1777\
 1778 # torchrec tests the code consistency with the following code

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_call\_impl(self, \*args, \*\*kwargs)\
 1785 or \_global\_backward\_pre\_hooks or \_global\_backward\_hooks\
 1786 or \_global\_forward\_hooks or \_global\_forward\_pre\_hooks):\
-> 1787 return forward\_call(\*args, \*\*kwargs)\
 1788\
 1789 result = None

/usr/local/lib/python3.12/dist-packages/transformers/utils/generic.py in wrapper(self, \*args, \*\*kwargs)\
 1000 outputs = func(self, \*args, \*\*kwargs)\
 1001 else:\
-> 1002 outputs = func(self, \*args, \*\*kwargs)\
 1003 except TypeError as original\_exception:\
 1004 # If we get a TypeError, it's possible that the model is not receiving the recordable kwargs correctly.

/usr/local/lib/python3.12/dist-packages/transformers/models/mistral3/modeling\_mistral3.py in forward(self, input\_ids, pixel\_values, attention\_mask, position\_ids, past\_key\_values, inputs\_embeds, vision\_feature\_layer, use\_cache, output\_attentions, output\_hidden\_states, return\_dict, cache\_position, image\_sizes, \*\*kwargs)\
 323 inputs\_embeds = inputs\_embeds.masked\_scatter(special\_image\_mask, image\_features)\
 324\
--> 325 outputs = self.language\_model(\
 326 attention\_mask=attention\_mask,\
 327 position\_ids=position\_ids,

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_wrapped\_call\_impl(self, \*args, \*\*kwargs)\
 1774 return self.\_compiled\_call\_impl(\*args, \*\*kwargs) # type: ignore\[misc\]\
 1775 else:\
-> 1776 return self.\_call\_impl(\*args, \*\*kwargs)\
 1777\
 1778 # torchrec tests the code consistency with the following code

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_call\_impl(self, \*args, \*\*kwargs)\
 1785 or \_global\_backward\_pre\_hooks or \_global\_backward\_hooks\
 1786 or \_global\_forward\_hooks or \_global\_forward\_pre\_hooks):\
-> 1787 return forward\_call(\*args, \*\*kwargs)\
 1788\
 1789 result = None

/usr/local/lib/python3.12/dist-packages/transformers/utils/generic.py in wrapper(self, \*args, \*\*kwargs)\
 1000 outputs = func(self, \*args, \*\*kwargs)\
 1001 else:\
-> 1002 outputs = func(self, \*args, \*\*kwargs)\
 1003 except TypeError as original\_exception:\
 1004 # If we get a TypeError, it's possible that the model is not receiving the recordable kwargs correctly.

/usr/local/lib/python3.12/dist-packages/transformers/models/mistral/modeling\_mistral.py in forward(self, input\_ids, attention\_mask, position\_ids, past\_key\_values, inputs\_embeds, use\_cache, cache\_position, \*\*kwargs)\
 395\
 396 for decoder\_layer in self.layers\[: self.config.num\_hidden\_layers\]:\
--> 397 hidden\_states = decoder\_layer(\
 398 hidden\_states,\
 399 attention\_mask=causal\_mask,

/usr/local/lib/python3.12/dist-packages/transformers/modeling\_layers.py in **call**(self, \*args, \*\*kwargs)\
 91\
 92 return self.\_gradient\_checkpointing\_func(partial(super().**call**, \*\*kwargs), \*args)\
---> 93 return super().**call**(\*args, \*\*kwargs)\
 94\
 95

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_wrapped\_call\_impl(self, \*args, \*\*kwargs)\
 1774 return self.\_compiled\_call\_impl(\*args, \*\*kwargs) # type: ignore\[misc\]\
 1775 else:\
-> 1776 return self.\_call\_impl(\*args, \*\*kwargs)\
 1777\
 1778 # torchrec tests the code consistency with the following code

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_call\_impl(self, \*args, \*\*kwargs)\
 1785 or \_global\_backward\_pre\_hooks or \_global\_backward\_hooks\
 1786 or \_global\_forward\_hooks or \_global\_forward\_pre\_hooks):\
-> 1787 return forward\_call(\*args, \*\*kwargs)\
 1788\
 1789 result = None

/usr/local/lib/python3.12/dist-packages/transformers/utils/generic.py in wrapped\_forward(\*args, \*\*kwargs)\
 953 if key == "hidden\_states" and len(collected\_outputs\[key\]) == 0:\
 954 collected\_outputs\[key\] += (args\[0\],)\
--> 955 output = orig\_forward(\*args, \*\*kwargs)\
 956 if not isinstance(output, tuple):\
 957 collected\_outputs\[key\] += (output,)

/usr/local/lib/python3.12/dist-packages/transformers/models/mistral/modeling\_mistral.py in forward(self, hidden\_states, attention\_mask, position\_ids, past\_key\_values, use\_cache, cache\_position, position\_embeddings, \*\*kwargs)\
 228 hidden\_states = self.input\_layernorm(hidden\_states)\
 229 # Self Attention\
--> 230 hidden\_states, \_ = self.self\_attn(\
 231 hidden\_states=hidden\_states,\
 232 attention\_mask=attention\_mask,

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_wrapped\_call\_impl(self, \*args, \*\*kwargs)\
 1774 return self.\_compiled\_call\_impl(\*args, \*\*kwargs) # type: ignore\[misc\]\
 1775 else:\
-> 1776 return self.\_call\_impl(\*args, \*\*kwargs)\
 1777\
 1778 # torchrec tests the code consistency with the following code

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_call\_impl(self, \*args, \*\*kwargs)\
 1785 or \_global\_backward\_pre\_hooks or \_global\_backward\_hooks\
 1786 or \_global\_forward\_hooks or \_global\_forward\_pre\_hooks):\
-> 1787 return forward\_call(\*args, \*\*kwargs)\
 1788\
 1789 result = None

/usr/local/lib/python3.12/dist-packages/transformers/models/mistral/modeling\_mistral.py in forward(self, hidden\_states, position\_embeddings, attention\_mask, past\_key\_values, cache\_position, \*\*kwargs)\
 151 hidden\_shape = (\*input\_shape, -1, self.head\_dim)\
 152\
--> 153 query\_states = self.q\_proj(hidden\_states).view(hidden\_shape).transpose(1, 2)\
 154 key\_states = self.k\_proj(hidden\_states).view(hidden\_shape).transpose(1, 2)\
 155 value\_states = self.v\_proj(hidden\_states).view(hidden\_shape).transpose(1, 2)

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_wrapped\_call\_impl(self, \*args, \*\*kwargs)\
 1774 return self.\_compiled\_call\_impl(\*args, \*\*kwargs) # type: ignore\[misc\]\
 1775 else:\
-> 1776 return self.\_call\_impl(\*args, \*\*kwargs)\
 1777\
 1778 # torchrec tests the code consistency with the following code

/usr/local/lib/python3.12/dist-packages/torch/nn/modules/module.py in \_call\_impl(self, \*args, \*\*kwargs)\
 1785 or \_global\_backward\_pre\_hooks or \_global\_backward\_hooks\
 1786 or \_global\_forward\_hooks or \_global\_forward\_pre\_hooks):\
-> 1787 return forward\_call(\*args, \*\*kwargs)\
 1788\
 1789 result = None

/usr/local/lib/python3.12/dist-packages/bitsandbytes/nn/modules.py in forward(self, x)\
 554 weight = self.weight if getattr(quant\_state, "packing\_format\_for\_cpu", False) else self.weight.t()\
 555\
--> 556 return bnb.matmul\_4bit(x, weight, bias=bias, quant\_state=quant\_state).to(inp\_dtype)\
 557\
 558

/usr/local/lib/python3.12/dist-packages/bitsandbytes/autograd/\_functions.py in matmul\_4bit(A, B, quant\_state, out, bias)\
 399 return out\
 400 else:\
--> 401 return MatMul4Bit.apply(A, B, out, bias, quant\_state)

/usr/local/lib/python3.12/dist-packages/torch/autograd/function.py in apply(cls, \*args, \*\*kwargs)\
 581 # See NOTE: \[functorch vjp and autograd interaction\]\
 582 args = \_functorch.utils.unwrap\_dead\_wrappers(args)\
--> 583 return super().apply(\*args, \*\*kwargs) # type: ignore\[misc\]\
 584\
 585 if not is\_setup\_ctx\_defined:

/usr/local/lib/python3.12/dist-packages/bitsandbytes/autograd/\_functions.py in forward(ctx, A, B, out, bias, quant\_state)\
 313 # 1. Dequantize\
 314 # 2. MatmulnN\
--> 315 output = torch.nn.functional.linear(A, F.dequantize\_4bit(B, quant\_state).to(A.dtype).t(), bias)\
 316\
 317 # 3. Save state

/usr/local/lib/python3.12/dist-packages/bitsandbytes/functional.py in dequantize\_4bit(A, quant\_state, absmax, out, blocksize, quant\_type)\
 1048 )\
 1049 else:\
-> 1050 out = torch.ops.bitsandbytes.dequantize\_4bit.default(\
 1051 A,\
 1052 absmax,

/usr/local/lib/python3.12/dist-packages/torch/\_ops.py in **call**(self, \*args, \*\*kwargs)\
 817 # that are named "self". This way, all the aten ops can be called by kwargs.\
 818 def **call**(self, /, \*args: \_P.args, \*\*kwargs: \_P.kwargs) -> \_T:\
--> 819 return self.\_op(\*args, \*\*kwargs)\
 820\
 821 # Use positional-only argument to avoid naming collision with aten ops arguments

/usr/local/lib/python3.12/dist-packages/torch/\_compile.py in inner(\*args, \*\*kwargs)\
 52 fn.\_\_dynamo\_disable = disable\_fn # type: ignore\[attr-defined\]\
 53\
---> 54 return disable\_fn(\*args, \*\*kwargs)\
 55\
 56 return inner

/usr/local/lib/python3.12/dist-packages/torch/\_dynamo/eval\_frame.py in \_fn(\*args, \*\*kwargs)\
 1179 ):\
 1180 return fn(\*args, \*\*kwargs)\
-> 1181 return fn(\*args, \*\*kwargs)\
 1182 finally:\
 1183 set\_eval\_frame(None)

/usr/local/lib/python3.12/dist-packages/torch/library.py in func\_no\_dynamo(\*args, \*\*kwargs)\
 740 [@torch](https://huggingface.co/torch) .\_disable\_dynamo\
 741 def func\_no\_dynamo(\*args, \*\*kwargs):\
--> 742 return func(\*args, \*\*kwargs)\
 743\
 744 for key in keys:

/usr/local/lib/python3.12/dist-packages/bitsandbytes/backends/cuda/ops.py in \_(A, absmax, blocksize, quant\_type, shape, dtype)\
 361 dtype: torch.dtype,\
 362 ) -> torch.Tensor:\
--> 363 out = torch.empty(shape, dtype=dtype, device=A.device)\
 364 \_dequantize\_4bit\_impl(A, absmax, blocksize, quant\_type, dtype, out=out)\
 365 return out

OutOfMemoryError: CUDA out of memory. Tried to allocate 40.00 MiB. GPU 0 has a total capacity of 14.56 GiB of which 17.81 MiB is free. Including non-PyTorch memory, this process has 14.54 GiB memory in use. Of the allocated memory 14.40 GiB is allocated by PyTorch, and 15.19 MiB is reserved by PyTorch but unallocated. If reserved but unallocated memory is large try setting PYTORCH\_ALLOC\_CONF=expandable\_segments:True to avoid fragmentation. See documentation for Memory Management ([https://pytorch.org/docs/stable/notes/cuda.html#environment-variables](https://pytorch.org/docs/stable/notes/cuda.html#environment-variables))
