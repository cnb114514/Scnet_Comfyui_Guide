
- 中文版(ZH)

# 国家超算互联网平台白嫖 GPU 运行 ComfyUI+Openclaw 指南
# Scnet ComfyUI baipiao Guide

[图文版教学](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide) [github guide(eng)](https://github.com/cnb114514/Scnet_Comfyui_Guide) 

## 📑 目录

- [准备](#准备)
- [1. 注册账号](#1-注册账号)
- [2. 白嫖资源](#2-白嫖资源)
- [3. 用社区镜像创建 Notebook容器](#3-用社区镜像创建-notebook容器)
- [4. 文件扩容](#4-文件扩容)
- [5. 访问 Linux 系统](#5-访问-linux-系统)
- [6. 启动 ComfyUI 后端程序](#6-启动-comfyui-后端程序)
- [7. 启动 ComfyUI 前端网页](#7-启动-comfyui-前端网页)
- [8. 关于模型下载](#8-关于模型下载)
- [9. ComfyUI 的正常操作](#9-comfyui-的正常操作)
- [10. 白嫖的 token 使用](#10-白嫖的-token-使用)
- [11. 问题反馈](#11-问题反馈)
- [12. 命令合集（一键快捷指令）](#12-命令合集一键快捷指令)

> 感谢 CNB 平台大佬 **cnb-xu**（又名 O.o）的开发，方便各位在国家投资的平台（十万张国产 GPU）上运行 ComfyUI。实测 海光bw 64gb显卡在没有任何加速的情况下，fp8效率为h20的1/4

> **提示**：部分 N 卡专用功能以及 LLM 暂不支持。

> **提示**：笔者并未测试过ssh方案，以下内容仅供参考。

## 准备

- **方案 A（推荐）**：一台能访问浏览器的电脑
- **方案 B**：如果通过通用云端 SSH 部署，请先安装以下两个文件到 Windows（适用于各种云平台的通用方案，例如仙宫云版本 O.o ComfyUI 镜像）：
  - [Icon.installericon.exe（PuTTY 安装包）](应用/Icon.installericon.exe)
  - [WinSCP-6.5.6-Setup.exe](应用/WinSCP-6.5.6-Setup%20(1).exe)

两个文件安装一下，安装向导里能勾选的功能都勾上即可。

——————————————————————————————————————————————

## 步骤

### 1. 注册账号

如果后续需要实名，就实名一下。

👉 [注册账号链接](https://www.scnet.cn/sso/register?service=https%3A%2F%2Fwww.scnet.cn%2Fac%2Fapi%2Fauth%2FloginSsoRedirect.action%3ForiginalUrl%3Dhttps%253A%252F%252Fwww.scnet.cn%252Fhome%252Fsubject%252Fhxjd%252Findex.html%253Fshow%253Dtrue%2526marketActivityId%253DgpQZWR9H%2526inviterId%253D11263962531)

——————————————————————————————————————————————

### 2. 白嫖资源

- 领取百万 token（7.17 当前为一个月的 kimi-2.6）：👉 [领取百万 token](https://www.scnet.cn/home/subject/hxjd/index.html)
- 领取国产 GPU（三个月）：👉 [领取国产 GPU](https://www.scnet.cn/ui/mall/search/resource?region=20091)

> **注意**：看到试用都能领，不过本教学用到的是 **BW1 64GB**。

![img2](图片/img2.png)

<sub>（步骤2图片）</sub>

——————————————————————————————————————————————

### 3. 用社区镜像创建 Notebook容器

容器管理界面（即人工智能）：👉 [容器管理界面](https://www.scnet.cn/ui/console/index.html#/notebook?notebookStatus=Running)

操作流程：

***创建 Notebook → 选择64gb显存的bw主机 → 社区镜像 → 搜索 o.o-comfyui 或 o.o-comfyui-withopenclaw → 选择最新的版本 → 部署***

> <sub>作者通道 [O.o-comfyui原版镜像库链接](https://www.scnet.cn/ui/aihub/image/scnet_xu/2077623634511740929) [O.o-comfyui-withopenclaw社区版镜像库链接](https://www.scnet.cn/ui/aihub/image/accma5g3vg/2078319083724169218)</sub>



> **提示**：每次启动可以运行 **72h**，操作完下一步 4文件扩容 可以保存关机！




![img3](图片/img3.png)

<sub>（步骤3图片）<sub>

> 等待启动完成……

——————————————————————————————————————————————

### 4. 文件扩容

平台**默认只给一台机器分配 500GB 空间**，但需要手动设置一下文件夹配额。

👉 [存储管理链接](https://www.scnet.cn/ui/console/index.html#/my-product/basic-resources/storage_resource)

操作步骤：

- 进入存储管理页面，点击 **扩容**；
- **用户主目录**：默认只有 50GB，改成 **450GB**；
- **共享目录**：设置为 **50GB**。

> <sub>共享目录暂时不清楚是干什么用的，先按 50GB 设置即可。</sub>

——————————————————————————————————————————————

### 5. 访问 Linux 系统

访问方式有两种：

- **方案 A（推荐）**：人工智能 Notebook 网页 JupyterLab 直接登录 👉 [JupyterLab 登录入口](https://www.scnet.cn/ui/console/index.html#/notebook)
- **方案 B（SSH）**：在人工智能页面，WinSCP 输入登录指令、密码、端口，直接连接机器。

> SSH 方法通用性强，可适用于各种云平台都玩的老资历。记得在「选项设置 → 应用设置」里保存密码，WinSCP 传输密码给 PuTTY，启动 ComfyUI 时就不用重复输入密码。

![img1](图片/img1.png)

<sub>（步骤5-B图片）</sub>

——————————————————————————————————————————————

### 6. 启动 ComfyUI 后端程序

访问 Linux 后，进入终端，输入 `qd` 回车（即 `/root/O.o-ai/qd` 的快捷方式）。

> **注意**：ComfyUI 本体的文件主目录并不在 `root` 的 `comfyui`，真正的工作区在 `private_data`，其下方才是 `comfyui`。

等待启动完成……可看到 **8188** 本地端口已创建。

——————————————————————————————————————————————

### 7. 启动 ComfyUI 前端网页

访问方案有三种：

- **方案 A**：执行完 5 和 6 后，在人工智能 Notebook 网页，容器右侧「访问自定义服务：8188」弹出隧道网页。
- **方案 B**：未执行 5 和 6，直接访问 自定义服务🔧（端口：8188，启动命令：`/root/O.o-ai/qd`）弹出隧道网页。
- **方案 C（SSH）**：执行完 5 和 6 后，通过 PuTTY 会把云端的本地端口映射为自己的本地端口，直接访问固定地址 👉 http://0.0.0.0:8188

——————————————————————————————————————————————

### 8. 关于模型下载

此镜像基于 cnb-xu 开发的自动下载方案：如果模型 Loader 节点下拉存在模型名字，运行工作流时会自动下载，速度 30–100 MB/s 不等。

- **方案 A（推荐!自定义下载）**：使用o.o-comfyui-withopenclaw社区镜像后，打开 `private_data/comfyui/自定义下载.yaml`，根据示例自己添加下载链接
- **方案 B（传统下载）**：选择 `private_data/comfyui/models`，进入对应模型类型的目录打开终端，用 `curl -L -OJ` 下载自己想跑的 CNB 模型。
- **方案 C（自动下载）**：直接打开工作流看看下拉菜单有没有模型，运行工作流即可。
- **方案 D（搬运自动下载）**：编辑 `private_data/comfyui/source.json`，把下载链接添加进去，或者添加本仓库里的两个文件，即可把 CNB 的社区收录搬运过去。👉 [aimodels包](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide/-/blob/auto/add-readme-0d40/cnb%E7%A4%BE%E5%8C%BA%E6%A8%A1%E5%9E%8B%E6%90%AC%E8%BF%90%E5%8C%85/source.json)
👉 [社区包](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide/-/blob/auto/add-readme-0d40/cnb%E7%A4%BE%E5%8C%BA%E6%A8%A1%E5%9E%8B%E6%90%AC%E8%BF%90%E5%8C%85/source-community.json)
- **方案 E（添加下载链接）**：终端输入 `ui`，再去人工智能 Notebook 网页，自定义端口改成 `7860`，就能在网页交互式下载模型。
- **方案 F（本地上传）**：直接拖拽上传到 JupyterLab 或 WinSCP，后者支持断点续传。

> 模型下载链接的获取支持 HuggingFace、CNB、ModelScope。

——————————————————————————————————————————————

### 9. ComfyUI 的正常操作

ComfyUI 的正常操作这里不再赘述，如果遇到问题可以问 AI，也可用白嫖的 token 新建 API 导入到 OpenClaw，让 AI 帮你解决问题。

——————————————————————————————————————————————

### 10. 白嫖的 token 使用

- **方案 A**：直接下载 scnet 的 Windows app，可免费调用 `scnet-max` / `kimi-2.6` 两种模型。
- **方案 B**：使用预装 openclaw 环境版本镜像 `O.o-comfyui-withopenclaw`，然后按下面操作：

  - 获取 API（需要先领取 kimi-2.6 token）：👉 [创建 apikey](https://www.scnet.cn/ui/console/index.html#/llm/apikeys)
  - 终端执行 `bash ~/setup_openclaw.sh` → 输入 api → 选择模型 → 回车。

——————————————————————————————————————————————

### 11. 问题反馈

- **渠道 1**：装完 openclaw 自己问。
- **渠道 2**：来 cnb 文档仓库的 issue 反馈 👉 [Scnet_Comfyui_Guide 仓库](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide) | [Issues 列表](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide/-/issues)
- **渠道 3**：加镜像作者 cnb-xu 的微信群 👉 [**群二维码**](https://cnb.cool/cnb-xu/docs/-/git/raw/main/comfyui/image/qrcode.png)

###### *cnb白嫖人，跑路！*

——————————————————————————————————————————————

### 12. 命令合集（一键快捷指令）

装好 `O.o-comfyui-withopenclaw` 镜像后，终端里直接敲下面这些指令即可，无需记复杂路径：

| 指令 | 作用说明 |
| :--- | :--- |
| `bash ~/setup_openclaw.sh` | ⬆️ 首次使用请先执行 `bash ~/setup_openclaw.sh`，按提示填入 scnet API 与模型，即可完成 OpenClaw 的环境配置。
| `openclaw` | 🚀启动openclaw (需预装openclaw版)
| `qd` | 🚀 启动comfyui（端口8188）
| `ui` | 🖥️ 启动交互模型下载器（端口7860） |
| `update` | ⬆️ 升级comfyui，推荐询问openclaw操作以保留魔改代码 |



> 💡 提示：以上指令均为镜像内置的快捷方式，建议在已完成 [步骤 5 访问 Linux 系统](#5-访问-linux-系统) 的终端中使用。

> ===================================================
# 更新日志
    **2026.07-17** o.o-comfyui-withopenclaw-1.0 新增openclaw预装和api导入交互脚本
    **2026.07-18** o.o-comfyui-withopenclaw-2.1 更新comfyui114514自动同步模型，aimodels模型，自定义下载模型脚本，cnb工作流示例（未安装插件）
###### *cnb白嫖人，跑路！*

# China National Supercomputing Internet (SCNet) GPU Platform ComfyUI + OpenClaw Deployment Guide
# Scnet ComfyUI baipiao Guide

==============================================================================================================================================================================

- ENGLISH(eng)


[图文版教学(zh)](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide) [github guide(eng)](https://github.com/cnb114514/Scnet_Comfyui_Guide) 


## 📑 Table of Contents

- [Prerequisites](#prerequisites)
- [1. Account Registration](#1-account-registration)
- [2. Claiming Free Resources](#2-claiming-free-resources)
- [3. Creating Notebook (Container Management)](#3-creating-notebook-container-management)
- [4. Storage Expansion](#4-storage-expansion)
- [5. Accessing the Linux System](#5-accessing-the-linux-system)
- [6. Starting ComfyUI Backend](#6-starting-comfyui-backend)
- [7. Accessing ComfyUI Frontend Web UI](#7-accessing-comfyui-frontend-web-ui)
- [8. Downloading Models](#8-about-model-downloading)
- [9. General ComfyUI Operations](#9-comfyui-general-operations)
- [10. Leveraging Free LLM Tokens](#10-leveraging-free-tokens)
- [11. Feedback & Community](#11-feedback--community)
- [12. Quick Commands Cheat Sheet](#12-command-list-one-key-shortcuts)

---

## Prerequisites

- **Option A (Recommended)**: A computer with a modern web browser.
- **Option B**: If deploying via SSH, download and install these Windows client utilities (general approach for many cloud-based instances, e.g., XianGong Cloud O.o ComfyUI image):
  - [Icon.installericon.exe (PuTTY Installer)](应用/Icon.installericon.exe)
  - [WinSCP-6.5.6-Setup.exe](应用/WinSCP-6.5.6-Setup%20(1).exe)

Simply run the setups on your local machine and check all optional components during installation.

---

## Steps

### 1. Account Registration

Complete real-name authentication if prompted.

👉 [Sign Up / Registration Link](https://www.scnet.cn/sso/register?service=https%3A%2F%2Fwww.scnet.cn%2Fac%2Fapi%2Fauth%2FloginSsoRedirect.action%3ForiginalUrl%3Dhttps%253A%252F%252Fwww.scnet.cn%252Fhome%252Fsubject%252Fhxjd%252Findex.html%253Fshow%253Dtrue%2526marketActivityId%253DgpQZWR9H%2526inviterId%253D11263962531)

---

### 2. Claiming Free Resources

- **Claim Million Tokens** (Currently features a free 1-month quota of Kimi-2.6): 👉 [Claim Tokens Portal](https://www.scnet.cn/home/subject/hxjd/index.html)
- **Claim Domestically-produced GPU** (3-month trial): 👉 [Claim GPU Portal](https://www.scnet.cn/ui/mall/search/resource?region=20091)

> **Note**: Free trial campaigns may vary. This tutorial explicitly targets the **BW1 64GB** GPU node.

![img2](图片/img2.png)
*(Step 2: Resource Allocation Marketplace)*

---

### 3. Creating Notebook (Container Management)

Navigate to the AI Container Console: 👉 [AI Notebook Console](https://www.scnet.cn/ui/console/index.html#/notebook?notebookStatus=Running)

**Step-by-step Setup:**
1. Click **Create Notebook**.
2. Select the host equipped with **BW1 64GB** VRAM.
3. Under the **Community Images** tab, search for: `o.o-comfyui` or `o.o-comfyui-withopenclaw`.
4. Choose the **latest version tag** and click **Deploy**.

> Author's Hub: [O.o-comfyui Base Image Hub](https://www.scnet.cn/ui/aihub/image/scnet_xu/2077623634511740929) | [O.o-comfyui-withopenclaw Community Image Hub](https://www.scnet.cn/ui/aihub/image/accma5g3vg/2078319083724169218)

> **Pro-Tip**: Active instances run up to **72 hours** per cycle. You can safely shut down (and preserve state) after completing [Step 4: Storage Expansion](#4-storage-expansion)!

![img3](图片/img3.png)
*(Step 3: Creating and Deploying AI Notebook)*

*Wait for status to transition to "Running"...*

---

### 4. Storage Expansion

By default, the platform allocates 500GB total workspace to each user, but you must manually configure directory quotas to utilize it properly.

👉 [Storage Management Panel](https://www.scnet.cn/ui/console/index.html#/my-product/basic-resources/storage_resource)

**Configuration Steps:**
- On the storage management interface, click **Resize/Expand (扩容)**.
- Set **User Home Directory (用户主目录)** to **450GB** (default is 50GB).
- Set **Shared Directory (共享目录)** to **50GB**.

*(Note: The Shared Directory usage is optional; setting it to 50GB works fine.)*

---

### 5. Accessing the Linux System

There are two primary methods to log into your workspace:

- **Option A (Recommended)**: Web-based entry via JupyterLab. Click "JupyterLab" next to your container in the console: 👉 [JupyterLab Portal](https://www.scnet.cn/ui/console/index.html#/notebook)
- **Option B (SSH)**: In the AI Container list, grab your SSH connection details (Host, Username, Port). Connect using WinSCP/PuTTY.

> The SSH method is extremely robust for seasoned developers. In WinSCP's Preferences, you can bind PuTTY so you don't have to enter the password repeatedly when launching multiple terminals.

![img1](图片/img1.png)
*(Step 5-B: Connecting via SSH clients)*

---

### 6. Starting ComfyUI Backend

Once inside the Linux Terminal:
1. Type `qd` and hit Enter (an alias shortcut for `/root/O.o-ai/qd`).
2. Wait for initialization. Once port **8188** is successfully bound, your backend is ready.

> **Note**: The main ComfyUI working directories do not live directly under `root/comfyui`. The active workspace is inside `/private_data/comfyui`.

---

### 7. Accessing ComfyUI Frontend Web UI

Choose one of three entry points:

- **Option A**: After launching via Step 5 & 6, click **"Custom Service (访问自定义服务): 8188"** in the SCNet AI Notebook panel to tunnel the UI to your browser.
- **Option B**: (No manual terminal start required) Configure Custom Service 🔧 (Port: `8188`, Start Command: `/root/O.o-ai/qd`) directly on the container dashboard, then open the exposed tunnel URL.
- **Option C (SSH Tunneling)**: After starting the backend, PuTTY automatically tunnels the remote port to your local machine. You can visit ComfyUI directly at 👉 http://127.0.0.1:8188

---

### 8. Downloading Models

This environment features an automated model acquisition daemon (by `cnb-xu`): if a model's name is populated in a loader node's dropdown list, running the workflow will trigger an automatic fast download at 30–100 MB/s.

- **Option A (Recommended - Custom Downloads)**: If using the `o.o-comfyui-withopenclaw` image, edit `/private_data/comfyui/自定义下载.yaml` to configure your own custom URLs.
- **Option B (Manual Download)**: Navigate to `/private_data/comfyui/models/<your-model-type>` in your terminal and run `curl -L -OJ <url>` to fetch raw models.
- **Option C (Auto-Fetch)**: Open a workflow, select a pre-indexed model from the loader's drop-down, and queue the prompt. The system pulls the model automatically.
- **Option D (Import CNB Repositories)**: Edit `/private_data/comfyui/source.json` or pull files from our guide repository to integrate CNB community model index maps.
  - 👉 [aimodels map](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide/-/blob/auto/add-readme-0d40/cnb%E7%A4%BE%E5%8C%BA%E6%A8%A1%E5%9E%8B%E6%90%AC%E8%BF%90%E5%8C%85/source.json)
  - 👉 [community map](https://cnb.cool/comfyui114514/Scnet_Comfyui_Guide/-/blob/auto/add-readme-0d40/cnb%E7%A4%BE%E5%8C%BA%E6%A8%A1%E5%9E%8B%E6%90%AC%E8%BF%90%E5%8C%85/source-community.json)
- **Option E (Web Downloader UI)**: Type `ui` in the terminal, then set your custom service mapping to port `7860`. You can download models interactively via a Web UI.
- **Option F (Local File Upload)**: Drag-and-drop uploads are supported directly inside JupyterLab or WinSCP (the latter supports resumable transfers).

*Downloader supports HuggingFace, CNB, and ModelScope sources.*

---

### 9. General ComfyUI Operations

Standard ComfyUI mechanics apply. If you run into issues, ask your integrated OpenClaw AI assistant or feed your free SCNet API key to leverage the assistant panel.

---

### 10. Leveraging Free LLM Tokens

- **Option A**: Use SCNet's official Windows client app to query `scnet-max` or `kimi-2.6` models directly.
- **Option B**: On the `O.o-comfyui-withopenclaw` image, configure the local autonomous companion:
  1. Generate your LLM API Key: 👉 [Create API Key Portal](https://www.scnet.cn/ui/console/index.html#/llm/apikeys)
  2. Run `bash ~/setup_openclaw.sh` in the terminal.
  3. Paste your key, select your preferred model, and hit Enter.

---

### 11. Feedback & Community

- **AI Companion**: Directly query OpenClaw locally in your environment.
- **Git Repository Issues**: 👉 [Scnet_Comfyui_Guide Issues](https://github.com/a1010580415-commits/Scnet_Comfyui_Guide/issues)
- **WeChat Community Group**: 👉 [WeChat Group QR Code](https://cnb.cool/cnb-xu/docs/-/git/raw/main/comfyui/image/qrcode.png)

*Farewell, CNB free tiers! Long live Supercomputing!* 🚀

---

### 12. Quick Commands Cheat Sheet

With `O.o-comfyui-withopenclaw` deployed, simply input these clean shorthand commands in the terminal:

| Command | Action Description |
| :--- | :--- |
| `bash ~/setup_openclaw.sh` | ⬆️ **First-time setup**: Paste your SCNet API key and model config to configure OpenClaw. |
| `openclaw` | 🚀 Boot the OpenClaw assistant (requires pre-installed version). |
| `qd` | 🚀 Boot ComfyUI backend server (Default Port: `8188`). |
| `ui` | 🖥️ Launch Web UI model downloader (Default Port: `7860`). |
| `update` | ⬆️ Upgrade ComfyUI core (ask OpenClaw first to preserve customized configs). |

> 💡 **Tip**: These shorthand shortcuts are built into the image shell. Ensure you have successfully connected to your terminal first ([Step 5](#5-accessing-the-linux-system)).

---

# Changelog

* **2026.07-17**: `o.o-comfyui-withopenclaw-1.0` - Initialized pre-configured OpenClaw instance with interactive API setup.
* **2026.07-18**: `o.o-comfyui-withopenclaw-2.1` - Updated automated model-sync mappings (aimodels), custom yaml parameters, and CNB default workflow presets.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



