pz-l8.md

192.168.10.1 or cmcc.wifi
6ny7jq@7

http://192.168.10.1/cgi-bin/luci

- [沛喆PZ-L8刷机总结](https://www.right.com.cn/forum/thread-8355384-1-1.html)

```
近日拿到一台沛喆PZ-L8，照着论坛上的kiss8888的沛喆PZ-L8-原版501.11 50.12固件及开SSH方法，无损刷集客等和 HowardWu 的沛喆PZ-L8 免拆，官方固件刷入OpenWrt方法刷机，折腾了一下午，对PZ-L8的刷机稍有心得，大概总结了一下，以方便其他坛友抄做。



开ssh方法
我之前是按沛喆L8 TTL刷集客，OpenWrt教程里用TTL连接的，但发现我这台机在跑码时不能中断，所以只能用以下方法。

1、检查路由器固件版本501.12，如不是，用“原厂50112固件.bin”刷机。（“原厂50112固件.bin”是kiss8888提供的501.12版官方固件，可去那里下载）
2、按住reset 20~30秒 不能超过 不能剪短 25秒最佳 在浏览器上打开192.168.10.1:56781网页
3、输入 root 和机器 背面的登录密码登录
4、输入 $(passwd${IFS}-d${IFS}root)    /清除root密码
5、输入 $(dropbear${IFS}-p${IFS}22)   /启动dropbear在22端口
6、输入 ssh -p 22 root@192.168.10.1  /ssh 运行在 22 端口
7、用putty连接登录ssh，192.168.10.1:22 登录名：root  无需密码
8、putty登录后，输入 vi /etc/config/dropbear   /修改dropbear配置文件
      找到 option enable       '0' 这一行，按I键，将“0”改成“1”然后按“ESC"键后输入 :wq 退出
9、输入 /etc/init.d/dropbear enable   这样dropbear就能随系统启动了，以后进SSH不用再按reset键从网页那里进去了。



刷op或集客固件，相关的固件可以去nwrt购买或去https://github.com/slienna/about-AX300M-WiFi6-ROUTER和http://file.cnrouter.com/index.php/Index/apbeta.html（找N3000）下载。
1、ssh 登录

2、输入： cat /proc/mtd                         看一眼 rootfs 对应的分区，一般是mtd15
2、输入： ubidetach -f -p /dev/mtdX     解锁rootfs 分区，X 换成 rootfs 对应的编号，比如 mtd15
3、上传要刷的固件至路由器的/tmp/目录下，建议将固件文件名改成1.bin 以便刷写
4、输入：ubiformat /dev/mtdX -y -f /tmp/1.bin   刷固件，X 换成 rootfs 对应的编号，比如 mtd15
5、刷入完成后拔电源重启就好，最好重启进系统后再在系统里升级刷一次固件。

注意：集客的刷完后要手动设电脑IP：6.6.6.7 掩码255.0.0.0 网关6.6.6.6，通过网页6.6.6.6进入集客AP管理页面进行更改设置集客的lan口地址方便后期管理。
如果失败就刷回官方再重试，反正有 uboot 只要不是作死在 uboot 刷入时断电死不了



从第三方固件刷回原官方固件的方法
uboot刷回
1、按住reset键通电，5秒后松开reset键，观察指示灯由明亮的红色变成微弱的红色就代表已进入uboot状态
2、电脑的网卡设置IP：192.168.10.2 掩码：255.255.255.0 网关 192.168.10.1
3、打开网页192.168.10.10，可以看到uboot界面
4、选择“原厂50112固件.bin”上传后刷机，耐心等待，自动重启后进入原官方管理界面192.168.10.1，登录密码就是设备原始贴后面的那个。


现有几个固件总结：
nwrt固件要收费，没购买，不知QSDK系统是否较快运行。
[color=var(--fgColor-accent, var(--color-accent-fg))]slienna的[color=var(--fgColor-default, var(--color-fg-default)) !important]PZL8-AX3000-openwrt-v1.6固件，wifi正常工作，5G可以工作在160Hz且不需手动设置。但管理页面反应迟钝，且功能较少，如需要自己安装插件又担心内存不够和拖慢系统。
集客AP，用的N3000的，跟集客其他AP一样功能，信号也一样。至于很多人说的VLAN不能正常工作，对于我来说，家庭用，VLAN没卵用。
最后，发现还是官方固件已够用，还是刷回官方固件了。

沛喆PZ-L8这个设备最大问题是信号不好，没有独立功放，隔堵墙，5G的信号下降40%~50%，太差了。过两天拿个移动RAX3000-QY再折腾一下看看。


以上开ssh和刷机方法均源于论坛，如有冒犯各位大神，敬请原谅。
谢谢！
```

- [沛喆PZ-L8-原版501.11 50.12固件及开SSH方法，无损刷集客等](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=8309403)

```
泸州产的升级完是501.11

深圳产的升级完是501.12
机器产地背面贴纸自行查看
深圳产的升级完可以直接在后台升级集客
注：升级固件一定要升级2次 或者升级完恢复出厂一次 要不然很容易自动退回版本

重要的事情说三遍  无论是在路由后台刷501.12版固件 或者是 刷集客固件 进系统后都需要恢复初始化一次 或者再刷一次 固定固件 因为L8有个恢复区
重要的事情说三遍  无论是在路由后台刷501.12版固件 或者是 刷集客固件 进系统后都需要恢复初始化一次 或者再刷一次 固定固件 因为L8有个恢复区

重要的事情说三遍  无论是在路由后台刷501.12版固件 或者是 刷集客固件 进系统后都需要恢复初始化一次 或者再刷一次 固定固件 因为L8有个恢复区


升级新版之后 按着reset 插电源 然后松开reset 进入192.168.10.10 可以进UBOOT 但是这里面不能刷集客或者OP 只能刷原版固件

开SSH 按住reset 20~30秒 不能超过 不能剪短 25秒最佳
网页打开192.168.10.1:56781
输入 root 和机器 背面的登录密码登录
输入命令
/usr/sbin/dropbear -F -P /var/run/dropbear.2.pid -p 22-K 300 -T 3 &

SSH就打开了 可以装shellCL······H等等
链接: https://pan.baidu.com/s/1Ms5XLJRzY1cv3cLRMCLFtg?pwd=1234 提取码: 1234 复制这段内容后打开百度网盘手机App，操作更方便哦
```



