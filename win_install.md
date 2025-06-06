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

WEPE ISO 2.3

https://www.bilibili.com/opus/958103252909424681

2.2


