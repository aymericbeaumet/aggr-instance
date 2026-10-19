---
title: 'From Brain Waves to Words: Brain2Qwerty Offers a New Path to Communication Without Surgery'
link: https://ai.meta.com/blog/brain2qwerty-brain-ai-human-communication/
source: ai-meta-com
published: 2026-06-29T00:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
content: extracted
html: 2026-06-29-from-brain-waves-to-words-brain2qwerty-offers-a-new-path-to.html
remote_preview:
  url: https://scontent-sjc6-1.xx.fbcdn.net/v/t39.2365-6/731147471_987177500749005_1892166673921917145_n.gif?_nc_cat=101&ccb=1-7&_nc_sid=e280be&_nc_ohc=I6lZV37XGj8Q7kNvwGOQRri&_nc_oc=AdoId63SXpSCX-nqQ2oE48d8KkNB9XOpbTUmYKtctQ03OopBBPCSwaFU0xakwp5A9RI&_nc_zt=14&_nc_ht=scontent-sjc6-1.xx&_nc_gid=vPdbxWJzwj5XTWluIdBYWA&_nc_ss=7b20f&oh=00_AQOXZhQ1aWYO6iVeJG54PQgJtUnRE5S89so3w6KCRPoEGQ&oe=6AEFC9ED
---

Last year, we introduced [Brain2Qwerty](https://ai.meta.com/blog/brain-ai-research-human-communication/) v1, research that uses AI to decode brain activity into text without any surgical implant. Now we're sharing the next step: Brain2Qwerty v2, the highest-performing end-to-end pipeline capable of real-time sentence decoding from non-invasive brain recordings, approaching levels of accuracy previously exclusive to techniques that require brain surgery.

To help accelerate neuroscience breakthroughs, we're releasing the full training code for Brain2Qwerty v1 and v2, and our partner, the Basque Center on Cognition, Brain, and Language (BCBL), is releasing the [v1 dataset](https://huggingface.co/datasets/bcbl190626/SpanishBCBL). We believe this research has the potential to make a real difference for the [millions of people](https://thejns.org/view/journals/j-neurosurg/130/4/article-p1080.xml) who suffer from brain lesions that prevent them from communicating. Invasive procedures like stereotactic electroencephalography and electrocorticography have shown that a neuroprosthesis feeding signals to an AI decoder can restore communication, but they're difficult to scale. Our non-invasive approach can help bridge that gap.

![](https://scontent-sjc6-1.xx.fbcdn.net/v/t39.8562-6/731199808_1542868887492186_7676795253949750980_n.png?_nc_cat=101&ccb=1-7&_nc_sid=f537c7&_nc_ohc=H9r8BD-9XyIQ7kNvwES1gLy&_nc_oc=AdplBdNYmMloSZE-qvfFygg5fr6Ccn_-ozH8Uo4fHL9vUoC_AaYD8XzajqvCs_-vUn8&_nc_zt=14&_nc_ht=scontent-sjc6-1.xx&_nc_gid=vPdbxWJzwj5XTWluIdBYWA&_nc_ss=7b20f&oh=00_AQOdSWrEH8C3qLpIODvKIyujLADl-gcBCf8h7_zHsMIrkQ&oe=6ADB36DC)

We trained Brain2Qwerty v2 on approximately 22,000 sentences from nine volunteer participants, each recorded for 10 hours wearing a magnetoencephalography (MEG) device while actively typing. Instead of relying on hand-crafted pipelines to detect neural events, we use end-to-end deep learning to decode directly from raw brain signals.

![](https://scontent-sjc3-1.xx.fbcdn.net/v/t39.2365-6/731219869_1076086811651166_7777400260549956173_n.png?_nc_cat=103&ccb=1-7&_nc_sid=e280be&_nc_ohc=DZzwpmRDJywQ7kNvwFt2Pdd&_nc_oc=Adq-YSC-JU0Bkn56KCdjkQI26kJ_pHc928ZQPPDjkhg9FFa3e_ip6RljWmz61Svm7Zo&_nc_zt=14&_nc_ht=scontent-sjc3-1.xx&_nc_gid=vPdbxWJzwj5XTWluIdBYWA&_nc_ss=7b20f&oh=00_AQMBjZcWHrozp0oLkTIbuMSyLfLKHWreU5tXP44qv6GmUw&oe=6AEF9E33)

Fine-tuning large language models on neural data allows the system to leverage semantic context, bridging the gap between noisy brain recordings and coherent language. We also deployed AI agents to explore optimizations for the decoding pipeline, with final training configurations selected manually by engineers.

![](https://scontent-sjc6-1.xx.fbcdn.net/v/t39.2365-6/731158975_1696243454915298_6767206741376253564_n.png?_nc_cat=100&ccb=1-7&_nc_sid=e280be&_nc_ohc=yEolp2gO8YAQ7kNvwGqQLfp&_nc_oc=AdrJFyT6VaE4vtl7ebTJdRNaWd_bblt-irooxpB-xOgiTvAAT_AnCdoKToEmIHDjlkk&_nc_zt=14&_nc_ht=scontent-sjc6-1.xx&_nc_gid=vPdbxWJzwj5XTWluIdBYWA&_nc_ss=7b20f&oh=00_AQOt6l9KXWhVYNVpSeIrMQIavtpQb3iiTAs-bL3l8DF6Pg&oe=6AEFB0B3)

The result: Brain2Qwerty v2 recovers sentences coherently from noisy neural inputs, achieving a word accuracy rate of 61%, significantly improving upon the 8% word accuracy from [other non-invasive methods](https://www.nature.com/articles/s41593-023-01304-9). And for our best participant, we achieve a 78% word accuracy, where more than half of all sentences are decoded with one word error or less.

We also find that decoding accuracy improves log-linearly with data volume, suggesting that the remaining performance gap with surgical approaches could be further narrowed through data scaling alone. This work contributes to our efforts to build open foundational models of the brain, with our [Tribev2 model](https://ai.meta.com/blog/tribe-v2-brain-predictive-foundation-model/) for perception encoding, [NeuralSet](https://arxiv.org/html/2605.03169v1) to process brain data at scale, and [NeuralBench](https://arxiv.org/abs/2605.08495) to systematically evaluate models. We do this in close collaboration with the community, through our recent $5 million fund to stimulate open datasets in our [Digital Brain Project](https://digitalbrainproject.org/). Our hope is that this work, done in the open, advances neuroscience to identify, diagnose, and treat neurological disorders faster than in siloes.
