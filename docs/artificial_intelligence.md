# Artificial Intelligence

## 前提摘要

> ⚠ 警告：如果您处于网络受限的国家或地区，请首先参阅 [网络配置篇章](./operations.md#huggingface)。

## ControlNet插件安装范例

前提：你要将AI绘画的WebUI更新到最新版本

将<https://github.com/Mikubill/sd-webui-controlnet>仓库中的内容拉取到WebUI文件目录下extensions文件夹内

也可以直接下载zip

下载该网站下的内容<https://huggingface.co/lllyasviel/ControlNet/tree/main/annotator/ckpts>，将其放到插件目录下的annotator下的ckpts目录

下载<https://huggingface.co/webui/ControlNet-modules-safetensors/tree/main>下的模型到插件目录下的models目录

现在你的ControlNet已经安装完毕

具体使用教程推荐B站视频<https://www.bilibili.com/video/BV1W>

> 来自 <https://zhuanlan.zhihu.com/p/607139523>  

## 提示词

```text
rori,full body shot,([cute:taiwan:0.5] girl,masterpeice,High detail RAW color photo, best quality, photograph,realistic),holding gun,m4a1,(6 years old),bright,petite,loli,sunglasses,mafia,underworld \(ornament\), 

Negative prompt: (((deformed))), blurry, bad anatomy, disfigured, poorly drawn face, mutation, mutated, (extra_limb), (ugly),(bad_prompt:0.8), (poorly drawn hands), fused fingers, messy drawing,anime,ugly,fat,small face, low quality,grayscale,watermark 

Steps: 32, Sampler: Euler a, CFG scale: 12, Seed: 1368638829, Face restoration: CodeFormer, Size: 512x768, Model hash: 990f4a980b, Model: 0.5(RealisticRori_v1.0_RealisticRori_v1.0_148688) + 0.5(chilloutmix_Ni), Clip skip: 2 
```

```text
rori,amine,doll,([cute:taiwan:0.5] girl,masterpeice,High detail RAW color photo, best quality, photograph,realistic),(6 years old),bright eyes,brown eyes,petite,loli,hanfu-3000,full body shot,hand on railing, railing, wood railing, trees,cherry blossoms, flower,looking away, pink flower 

Negative prompt: (((deformed))), blurry, bad anatomy, disfigured, poorly drawn face, mutation, mutated, (extra_limb), (ugly),(bad_prompt:0.8), (poorly drawn hands), fused fingers, messy drawing,anime,ugly,fat,small face, low quality,grayscale,watermark,looking at viewer, 

Steps: 32, Sampler: Euler a, CFG scale: 12, Seed: 2873750764, Face restoration: CodeFormer, Size: 768x512, Model hash: 990f4a980b, Model: 0.5(RealisticRori_v1.0_RealisticRori_v1.0_148688) + 0.5(chilloutmix_Ni), Clip skip: 2 
```
