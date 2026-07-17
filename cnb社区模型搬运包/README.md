# cnb社区模型搬运包

本文件夹用于存放 ComfyUI 模型自动下载方案所需的 `source.json` 配置文件。

## 文件说明

| 文件 | 说明 |
| --- | --- |
| `source.json` | 官方/主流模型源配置，包含 Comfy-Org、comfyanonymous、Kijai、Lightricks、black-forest-labs、unsloth、各类社区作者等大量模型的下载路径映射。 |
| `source-community.json` | 社区收录的额外模型源配置（如 O.o-AI、AIxcnb、SKDZSS90、goooojt、fuliai、comfyui114514、chisaki-aigc 等）。 |

## 使用方法

参考主仓库 README「步骤 8 - 关于模型下载」中的 **方案 C（添加自动下载）**：

1. 将本文件夹中的 `source.json` 与 `source-community.json` 上传到镜像内的 `private_data/comfyui/` 目录（覆盖或合并原文件）。
2. 之后在 ComfyUI 工作流中，Loader 节点下拉即可看到对应模型名称，运行工作流时会自动从 CNB 下载（速度 30–100 MB/s 不等）。

> 这样即可把 CNB 社区收录的模型搬运到自己的 ComfyUI 环境中使用。
