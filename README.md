# Scnet ComfyUI Guide
感谢cnb平台大佬cnb-xu的开发（又名O.o），方便各位在国家投资的平台十万张国产gpu运行comfyui

准备：方案A一台能访问浏览器的电脑/方案B 如果通过通用云端ssh部署，请先安装以下两个文件到windows（适用于各种云平台的通用方案，例如仙宫云版本O.o comfyui镜像）
putty-64bit-0.84-installer.msi
WinSCP-6.5.6-Setup.exe
两个文件安装一下并且里面能装功能的都勾上
提示：部分n卡专用功能以及llm暂不支持

步骤
1
注册账号，如果后续需要实名的就实名一下
https://www.scnet.cn/sso/register?service=https%3A%2F%2Fwww.scnet.cn%2Fac%2Fapi%2Fauth%2FloginSsoRedirect.action%3ForiginalUrl%3Dhttps%253A%252F%252Fwww.scnet.cn%252Fhome%252Fsubject%252Fhxjd%252Findex.html%253Fshow%253Dtrue%2526marketActivityId%253DgpQZWR9H%2526inviterId%253D11263962531

2
领取百万token（7.17 当前为一个月的kimi-2.6）
https://www.scnet.cn/home/subject/hxjd/index.html
领取国产gpu（三个月）
https://www.scnet.cn/ui/mall/search/resource?region=20091
都能领，不过本教学用到的是BW1 64gb

3
https://www.scnet.cn/ui/console/index.html#/notebook?notebookStatus=Running

这里是容器管理界面（即人工智能）
创建notebook-社区镜像-搜索O.o-comfyui-选择版本-部署
（这是O.o-comfyui对应镜像库链接，作者暂时没有写文档https://www.scnet.cn/ui/aihub/image/scnet_xu/2077623634511740929）

4 暂未明确如何操作，可跳过
据说 人工智能界面还有一个文件管理，用户主目录记得调大一些，总共500gb硬盘可分配
笔者进入后啥也看不到

5