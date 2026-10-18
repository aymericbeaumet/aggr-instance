---
title: 'Qwen-Image-Layered: Layered Decomposition for Inherent Editablity'
link: https://qwen.ai/blog?id=qwen-image-layered
source: qwen-ai
published: 2025-12-19T05:08:30Z
first_seen: 2026-10-18T23:27:08.867657739Z
authors:
- QwenTeam
labels:
- research
content: feed
html: 2025-12-19-qwen-image-layered-layered-decomposition-for-inherent.html
remote_preview:
  url: https://img.alicdn.com/imgextra/i2/O1CN01y9LKxE248WbyrZzFw_!!6000000007346-0-tps-1590-954.jpg
---

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/layered.JPG)

[QWEN CHAT](https://qwen.ai) [GITHUB](https://github.com/QwenLM/Qwen-Image-Layered) [HUGGING FACE](https://huggingface.co/Qwen/Qwen-Image-Layered) [MODELSCOPE](https://modelscope.cn/models/Qwen/Qwen-Image-Layered) [DEMO](https://https://huggingface.co/spaces/Qwen/Qwen-Image-Layered)

Today, we are excited to introduce Qwen-Image-Layered, a model capable of decomposing an image into multiple RGBA layers. This layered representation unlocks inherent editability: each layer can be independently manipulated without affecting other content. Meanwhile, such a layered representation naturally supports high-fidelity elementary operations-such as resizing, reposition, and recoloring. By physically isolating semantic or structural components into distinct layers, our approach enables high-fidelity and consistent editing.

## Layered Decomposition in Application

Given an image, Qwen-Image-Layered can decompose it into several RGBA layers:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%871.JPG#center%20)

After decomposition, edits are applied exclusively to the target layer, physically isolating it from the rest of the content, and thereby fundamentally ensuring consistency across edits.

For example, we can recolor the first layer and keep all other content untouched:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%872.JPG#center%20)

We can also replace the second layer from a girl to a boy:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%873.JPG#center%20)

Here, we revise the text to “Qwen-Image”:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%874.JPG#center%20)

Furthermore, the layered structure naturally supports elemetary operations. For example, we can delete unwanted objects cleanly:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%875.JPG#center%20)

We can also resize an object without distortion:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%876.JPG#center%20)

After layer decomposition, we can move objects freely within the canvas:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%877.JPG#center%20)

## Flexible and Iterative Decomposition

Qwen-Image-Layered is not limited to a fixed number of layers. The model supports variable-layer decomposition. For example, we can decompose an image into either 3 or 8 layers as needed:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%878.JPG#center%20)

Moreover, decomposition can be applied recursively: any layer can itself be further decomposed, enabling infinite decomposition.

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/layered/%E5%B9%BB%E7%81%AF%E7%89%879.JPG#center%20)

## Conclusion

Qwen-Image-Layered bridges the gap between raster imagery and structured, editable representations. By reimagining images as composable layers, we hope to enable intuitive, precise, and robust editing capabilities.

## Citation

If you find our model useful in your research, please consider citing us 📝 :)

```bibtex
@misc{yin2025qwenimagelayered,
      title={Qwen-Image-Layered: Towards Inherent Editability via Layer Decomposition}, 
      author={Shengming Yin, Zekai Zhang, Zecheng Tang, Kaiyuan Gao, Xiao Xu, Kun Yan, Jiahao Li, Yilei Chen, Yuxiang Chen, Heung-Yeung Shum, Lionel M. Ni, Jingren Zhou, Junyang Lin, Chenfei Wu},
      year={2025},
      eprint={2512.15603},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2512.15603}, 
}
```
