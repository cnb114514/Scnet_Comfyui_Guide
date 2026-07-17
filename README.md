# 国家超算互联网平台白嫖 GPU 运行 ComfyUI 指南（Scnet ComfyUI baipiao Guide）

感谢 CNB 平台大佬 **cnb-xu**（又名 O.o）的开发，方便各位在国家投资的平台（十万张国产 GPU）上运行 ComfyUI。

> **提示**：部分 N 卡专用功能以及 LLM 暂不支持。

> **提示**：笔者并未测试过ssh方案，以下内容仅供参考。

## 准备

- **方案 A（推荐）**：一台能访问浏览器的电脑
- **方案 B**：如果通过通用云端 SSH 部署，请先安装以下两个文件到 Windows（适用于各种云平台的通用方案，例如仙宫云版本 O.o ComfyUI 镜像）：
  - [Icon.installericon.exe（PuTTY 安装包）](应用/Icon.installericon.exe)
  - [WinSCP-6.5.6-Setup.exe](应用/WinSCP-6.5.6-Setup%20(1).exe)

两个文件安装一下，安装向导里能勾选的功能都勾上即可。

---

## 步骤

### 1. 注册账号

如果后续需要实名，就实名一下。

👉 [注册账号链接](https://www.scnet.cn/sso/register?service=https%3A%2F%2Fwww.scnet.cn%2Fac%2Fapi%2Fauth%2FloginSsoRedirect.action%3ForiginalUrl%3Dhttps%253A%252F%252Fwww.scnet.cn%252Fhome%252Fsubject%252Fhxjd%252Findex.html%253Fshow%253Dtrue%2526marketActivityId%253DgpQZWR9H%2526inviterId%253D11263962531)

### 2. 白嫖资源

- 领取百万 token（7.17 当前为一个月的 kimi-2.6）：👉 [领取百万 token](https://www.scnet.cn/home/subject/hxjd/index.html)
- 领取国产 GPU（三个月）：👉 [领取国产 GPU](https://www.scnet.cn/ui/mall/search/resource?region=20091)

> **注意**：看到试用都能领，不过本教学用到的是 **BW1 64GB**。

![img2](图片/img2.png)

### 3. 创建 Notebook（容器管理）

容器管理界面（即人工智能）：👉 [容器管理界面](https://www.scnet.cn/ui/console/index.html#/notebook?notebookStatus=Running)

操作流程：

> 创建 Notebook → 社区镜像 → 搜索 **O.o-comfyui** → 选择版本 → 部署

（注：O.o-comfyui 对应镜像库链接，作者暂时没有写文档，不重要：[O.o-comfyui镜像库链接](https://www.scnet.cn/ui/aihub/image/scnet_xu/2077623634511740929)）

等待启动完成……

每次启动可以运行 **72h**。

### 4. 文件管理（暂未明确如何操作，可跳过）

据说人工智能界面还有一个文件管理，用户主目录记得调大一些，总共 500GB 硬盘可分配。

> 笔者进入后啥也看不到，故暂略。

### 5. 访问 Linux 系统

访问方式有两种：

- **方案 A（推荐）**：人工智能 Notebook 网页 JupyterLab 直接登录 👉 [JupyterLab 登录入口](https://www.scnet.cn/ui/console/index.html#/notebook)
- **方案 B（SSH）**：在人工智能页面，WinSCP 输入登录指令、密码、端口，直接连接机器。

> SSH 方法通用性强，可适用于各种云平台都玩的老资历。记得在「选项设置 → 应用设置」里保存密码，WinSCP 传输密码给 PuTTY，启动 ComfyUI 时就不用重复输入密码。

![img1](图片/img1.png)

### 6. 启动 ComfyUI 后端程序

访问 Linux 后，进入终端，输入 `qd` 回车（即 `/root/O.o-ai/qd` 的快捷方式）。

> **注意**：ComfyUI 本体的文件主目录并不在 `root` 的 comfyui，真正的工作区在 `private_data`，下方有 comfyui。

等待启动完成……可看到 **8188** 本地端口已创建。

### 7. 启动 ComfyUI 前端网页

访问方案有三种：

- **方案 A**：执行完 5 和 6 后，在人工智能 Notebook 网页，容器右侧「访问自定义服务：8188」弹出隧道网页。
- **方案 B**：未执行 5 和 6，直接访问自定义服务（端口：8188，启动命令：`/root/O.o-ai/qd`）弹出隧道网页。
- **方案 C（SSH）**：执行完 5 和 6 后，通过 PuTTY 会把云端的本地端口映射为自己的本地端口，直接访问固定地址 👉 http://0.0.0.0:8188

### 8. 关于模型下载

此镜像基于 cnb-xu 开发的自动下载方案：如果模型 Loader 节点下拉存在模型名字，运行工作流时会自动下载，速度 30–100 MB/s 不等。

- **方案 A（传统下载）**：选择 `private_data/comfyui/models`，进入对应模型类型的目录打开终端，用 `curl -L -OJ` 下载自己想跑的 CNB 模型。
- **方案 B（自动下载）**：直接打开工作流看看下拉菜单有没有模型，运行工作流即可。
- **方案 C（添加自动下载）**：编辑 `private_data/comfyui/source.json`，把下载链接添加进去，或者添加本仓库里的两个文件，即可把 CNB 的社区收录搬运过去。
- **方案 D（添加下载链接）**：终端输入 `ui`，再去人工智能 Notebook 网页，自定义端口改成 `7860`，就能在网页交互式下载模型。
- **方案 E（本地上传）**：直接拖拽上传到 JupyterLab 或 WinSCP，后者支持断点续传。

> 模型下载链接的获取支持 HuggingFace、CNB、ModelScope。

### 9. ComfyUI 的正常操作

ComfyUI 的正常操作这里不再赘述，如果遇到问题可以问 AI，也可用白嫖的 token 新建 API 导入到 OpenClaw，让 AI 帮你解决问题。

> （预装 OpenClaw 的版本开发中……）


###### *cnb白嫖人，跑路！*