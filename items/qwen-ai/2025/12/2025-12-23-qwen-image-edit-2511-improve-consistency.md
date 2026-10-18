---
title: 'Qwen-Image-Edit-2511: Improve Consistency'
link: https://qwen.ai/blog?id=qwen-image-edit-2511
source: qwen-ai
published: 2025-12-23T05:08:30Z
first_seen: 2026-10-18T23:27:08.867657739Z
authors:
- QwenTeam
labels:
- open-source
content: feed
html: 2025-12-23-qwen-image-edit-2511-improve-consistency.html
remote_preview:
  url: https://img.alicdn.com/imgextra/i1/O1CN015JyH0e1jXcNfdFALl_!!6000000004558-0-tps-1590-954.jpg
---

![](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/Qwen-Image/edit2511/edit2511big.JPG#center)

[QWEN CHAT](https://chat.qwen.ai/?inputFeature=image_edit) [GITHUB](https://github.com/QwenLM/Qwen-Image) [HUGGING FACE](https://huggingface.co/Qwen/Qwen-Image-Edit-2511) [MODELSCOPE](https://modelscope.cn/models/Qwen/Qwen-Image-Edit-2511) [DISCORD](https://discord.gg/yPEP2vHTu4)

We are excited to introduce Qwen-Image-Edit-2511, an enhanced version over Qwen-Image-Edit-2509, featuring multiple improvements—including notably better consistency. To try out the latest model, please visit [Qwen Chat](https://chat.qwen.ai/?inputFeature=image_edit) and select the Image Editing feature. Note that the online version includes certain optimizations for speed; for the best possible performance, we recommend deploying the model locally via [ModelScope](https://modelscope.cn/models/Qwen/Qwen-Image-Edit-2511).

Key enhancements in Qwen-Image-Edit-2511 include: mitigate image drift, improved character consistency，integrated LoRA capabilities， enhanced industrial design generation, and strengthened geometric reasoning ability.

## Showcase Examples

**Qwen-Image-Edit-2511 Enhances Character Consistency** In Qwen-Image-Edit-2511, character consistency has been significantly improved. The model can perform imaginative edits based on an input portrait while preserving the identity and visual characteristics of the subject.

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%871.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%872.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%873.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%874.JPG#center%20)

**Improved Multi-Person Consistency** While Qwen-Image-Edit-2509 already improved consistency for single-subject editing, Qwen-Image-Edit-2511 further enhances consistency in multi-person group photos—enabling high-fidelity fusion of two separate person images into a coherent group shot:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%875.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%876.JPG#center%20)

**Built-in Support for Community-Created LoRAs** Since Qwen-Image-Edit’s release, the community has developed many creative and high-quality LoRAs—greatly expanding its expressive potential. Qwen-Image-Edit-2511 integrates selected popular LoRAs directly into the base model, unlocking their effects without extra tuning.

For example, Lighting Enhancement LoRA Realistic lighting control is now achievable out-of-the-box:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%877.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%878.JPG#center%20)

Another example, generating new viewpoints can now be done directly with the base model:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%879.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%8710.JPG#center%20)

**Industrial Design Applications**

We’ve paid special attention to practical engineering scenarios—for instance, batch industrial product design:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%8711.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%8712.JPG#center%20)

…and material replacement for industrial components:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%8713.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%8714.JPG#center%20)

**Enhanced Geometric Reasoning** Qwen-Image-Edit-2511 introduces stronger geometric reasoning capability—e.g., directly generating auxiliary construction lines for design or annotation purposes:

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%8715.JPG#center%20)

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/Qwen-Image/edit2511/%E5%B9%BB%E7%81%AF%E7%89%8716.JPG#center%20)

That wraps up the major updates in Qwen-Image-Edit-2511. Enjoy exploring the new capabilities! 🎉

## Citation

If you find our model useful in your research, please consider citing us 📝 :)

```bibtex
@misc{wu2025qwenimagetechnicalreport,
      title={Qwen-Image Technical Report}, 
      author={Chenfei Wu and Jiahao Li and Jingren Zhou and Junyang Lin and Kaiyuan Gao and Kun Yan and Sheng-ming Yin and Shuai Bai and Xiao Xu and Yilei Chen and Yuxiang Chen and Zecheng Tang and Zekai Zhang and Zhengyi Wang and An Yang and Bowen Yu and Chen Cheng and Dayiheng Liu and Deqing Li and Hang Zhang and Hao Meng and Hu Wei and Jingyuan Ni and Kai Chen and Kuan Cao and Liang Peng and Lin Qu and Minggang Wu and Peng Wang and Shuting Yu and Tingkun Wen and Wensen Feng and Xiaoxiao Xu and Yi Wang and Yichang Zhang and Yongqiang Zhu and Yujia Wu and Yuxuan Cai and Zenan Liu},
      year={2025},
      eprint={2508.02324},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2508.02324}, 
}
```
