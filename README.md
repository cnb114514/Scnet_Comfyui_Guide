# 国家超算互联网平台白嫖gpu运行comfyui指南 Scnet ComfyUI Guide
感谢cnb平台大佬cnb-xu的开发（又名O.o），方便各位在国家投资的平台十万张国产gpu运行comfyui

准备：方案A一台能访问浏览器的电脑/方案B 如果通过通用云端ssh部署，请先安装以下两个文件到windows（适用于各种云平台的通用方案，例如仙宫云版本O.o comfyui镜像）
putty-64bit-0.84-installer.msi
WinSCP-6.5.6-Setup.exe
两个文件安装一下并且里面能装功能的都勾上
提示：部分n卡专用功能以及llm暂不支持

步骤
1 注册账号
如果后续需要实名的就实名一下
https://www.scnet.cn/sso/register?service=https%3A%2F%2Fwww.scnet.cn%2Fac%2Fapi%2Fauth%2FloginSsoRedirect.action%3ForiginalUrl%3Dhttps%253A%252F%252Fwww.scnet.cn%252Fhome%252Fsubject%252Fhxjd%252Findex.html%253Fshow%253Dtrue%2526marketActivityId%253DgpQZWR9H%2526inviterId%253D11263962531

2 白嫖
领取百万token（7.17 当前为一个月的kimi-2.6）
https://www.scnet.cn/home/subject/hxjd/index.html
领取国产gpu（三个月）
https://www.scnet.cn/ui/mall/search/resource?region=20091
注：看到试用都能领，不过本教学用到的是BW1 64gb

3
https://www.scnet.cn/ui/console/index.html#/notebook?notebookStatus=Running

这里是容器管理界面（即人工智能）
创建notebook-社区镜像-搜索O.o-comfyui-选择版本-部署
（注：O.o-comfyui对应镜像库链接，作者暂时没有写文档，不重要 https://www.scnet.cn/ui/aihub/image/scnet_xu/2077623634511740929）

等待启动完成...
每次启动可以运行72h

4 >>>暂未明确如何操作，可跳过
据说 人工智能界面 还有一个文件管理，用户主目录记得调大一些，总共500gb硬盘可分配
笔者进入后啥也看不到

5 访问linux系统
访问的方式有两种，
方案A 人工智能notebook网页 jupyterlab直接登录（推荐） https://www.scnet.cn/ui/console/index.html#/notebook
方案B ssh方案在人工智能页面 winscp输入登录指令密码端口直接连接机器
 ----ssh方法通用型强，可以适用于各种云平台都玩的老资历，记得在选项设置里，应用设置，保存密码，winscp传输密码给putty启动comfyui时就不用重复输入密码

6 启动comfyui后端程序
访问linux后，进入终端，输入qd回车（即/root/O.o-ai/qd的快捷方式），
---------------------注：comfyui本体的文件的主目录并不在root的comfyui，真正的工作区在private_data，下方有comyui
等待启动完成...可看到8188本地端口已创建

7 启动comfyui前端网页
访问的方案有三种
方案A 执行完5和6后，在人工智能notebook网页，容器右侧 访问自定义服务：8188 弹出隧道网页
方案B 未执行5和6，直接访问自定义服务 端口：8188 启动命令：/root/O.o-ai/qd 弹出隧道网页
方案C SSH方案执行5和6后通过putty会把云端的本地端口变成自己的本地端口，直接访问固定地址http://0.0.0.0:8188

8 关于模型下载
此镜像基于cnb-xu开发的自动下载方案，如果模型loader节点下拉存在模型名字，运行工作流时会自动下载，速度30-100mb/s不等
方案A 传统下载 选择private_data-comfyui-models然后对应模型类型的目录打开终端，用curl -L -OJ下载自己想跑的cnb模型
方案B 自动下载 直接打开工作流看看下拉菜单有没有模型，运行工作流即可
方案C 添加自动下载 private_data-comfyui-source.json 把下载链接添加进去 或者添加本仓库里的两个文件即可把cnb的社区收录搬运过去
方案D 添加下载链接 终端输入ui 再去人工智能note网页，自定义端口改成7860，就能在网页交互式下载模型
方案E 本地上传 直接拖拽上传到jupyterlab或者winscp，后者支持断点续传
模型下载链接的获取支持huggingface，cnb，modelscope

9 comfyui的正常操作这里不再赘述，如果遇到问题可以问ai，也可也用白嫖的token新建api导入到openclaw让ai帮你解决问题
（预装openclaw的版本开发中...)

