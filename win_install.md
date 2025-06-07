# win install

# Ventoy + WEPE iso + WIN ISO + HD3000 driver

https://www.reddit.com/r/lowendgaming/comments/1258usx/updated_win1011_drivers_for_intel_hd_3000/

============================================

https://massgrave.dev/windows_ltsc_links

# win 10 activation

https://github.com/NiREvil/windows-activation

```
irm https://get.activated.win | iex
```


====================================================

Proxy

https://chromewebstore.google.com/detail/proxy-switchyomega-v3/hihblcmlaaademjlakdpicchbjnnnkbo
====================================================
最好的KMS激活Windows和office网站v0v官方说明 https://www.weizhiyong.com/archives/1896 

[本站永久地址] https://v0v.bid

```

1.Windows 激活方法
1.打开 命令提示符 (管理员)

开始菜单-搜索“cmd”-找到“命令提示符”-右键“以管理员身份运行”。

2.执行以下命令（复制命令-右键粘贴）

slmgr /skms kms.v0v.bid && slmgr /ato

大部分情况下 你能下载到的系统镜像都是VOL版（Business版）仅需以上一步即可成功激活。

查看系统版本（备用）
wmic os get caption
查看激活详情（备用）
slmgr /dlv
查看所有命令（备用）
slmgr
本站同时提供了备用的便捷激活脚本（备用）跳转下文


附1：Windows 的 KMS 激活密钥（Product GVLK）
https://docs.microsoft.com/zh-cn/windows-server/get-started/kms-client-activation-keys
Windows Server 2022（LTSC 版本）
Windows Server 2022 Datacenter
WX4NM-KYWYW-QJJR4-XV3QB-6VM33
```


========================


https://anal02.pixnet.net/blog/post/121378914

WINDOWS10 “Traditional Chinese IME is not ready yet”

這方式成功，剛更新完windows的痛

打開CMDimage

輸入   DISM.exe /Online /Add-Capability /CapabilityName:Language.Basic~~~zh-TW~0.0.1.0

image

注意要使用最高權限

台灣繁中:

DISM.exe /Online /Add-Capability /CapabilityName:Language.Basic~~~zh-TW~0.0.1.0

香港繁中:

DISM.exe /Online /Add-Capability /CapabilityName:Language.Basic~~~zh-HK~0.0.1.0

日文可以試試這個指令:

DISM.exe /Online /Add-Capability /CapabilityName:Language.Basic~~~ja-JP~0.0.1.0

=============================

Language suport

https://blog.samsam123.name.my/articles/windows-11--22h2-23h2-24h2-install-language-pack-via-dism

https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/virtual-desktop/windows-11-language-packs.md

```
微软官方提供的 Features on Demand (FOD) 镜像，并且通过 DISM 安装这些语言包，等同于可以离线安装这些语言包了！
https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/virtual-desktop/windows-11-language-packs.md

从上面提供的 OneDrive 链接下载 Microsoft-Windows-LanguageFeatures-Basic-zh-cn-Package~31bf3856ad364e35~amd64_24H2.cab 文件

请注意：这个文件仅支持 24H2 版本 , 23H2 / 22H2 需要下载 22H2-23H2-FOD-Lang.zip ，错误下载会导致DISM安装时报错 0x800f0818 


然后，用管理员权限打开 Powershell ， 并输入如下指令 :

dism /Online /Add-Package /PackagePath:/path/to/cab

请替换 /path/to/cab 为正确路径 

安装需用时 5-10分钟，请耐心等候. 安装成功后有概率要求重启电脑，请务必配合提示操作！


安装成功后可以尝试输入法是否可以正常使用啦！

```
=============================

https://zhuanlan.zhihu.com/p/598577028

```
二、Appx方式；
首先怎么得到appx格式的简体中文包让我费劲周折，最后受@Gravitation此贴启发：


1、打开https://apps.microsoft.com/store/apps微软官方商店，搜索简体中文，得到Productld如下图红色箭头所示9NRMNT6GMZ70；

https://apps.microsoft.com/detail/9pcj4dhcq1jq?hl=en-US&gl=US

2、打开https://store.rg-adguard.net，选择Productld，输入9NRMNT6GMZ70，选择retail，最后找到最新版22621开头，离线下载appx包；

https://store.rg-adguard.net/

3、将离线包复制到C盘根目录下，打开PowerShell，切换到根目录，Add-AppxPackage开始安装；

cd /

Add-AppxPackage Microsoft.LanguageExperiencePackzh-CN_22621.11.66.0_neutral__8wekyb3d8bbwe.Appx

4、重启电脑，然后开始如下图一开始切换语言，提示退出或者注销生效， 然后进入桌面后，下图二4处均改为简体中文，汉化就基本全部完成；
```

=============================

WEPE ISO 2.3

https://www.bilibili.com/opus/958103252909424681

2.2

================================
# VHD

https://blog.csdn.net/weixin_43465752/article/details/145408342

https://post.smzdm.com/p/aenl7gkq/

创建虚拟磁盘、写入系统文件、添加引导。

windows + X

https://www.bilibili.com/opus/583475361619691963

软件准备
-winNTsetup（用于部署Windows11系统）

-dism++（用于备份当前驱动）

-BOOTICE（用于添加虚拟磁盘启动项）

# VHD + Ventoy

https://www.techbang.com/posts/93725-windows-11-to-go
