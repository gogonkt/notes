# kisslinx_install.md

Pre-
@root
```
fdisk /dev/mmcblk0
  P1 +500M ef fat32 boot
  P2 +20G ext4
modprobe vfat
mkfs.vfat /dev/mmcblk0p1
mkfs.ext4 /dev/mmcblk0p2
```




ref:
https://kisscommunity.org/kiss/install/
https://kisslinux.org/install.html
https://github.com/mcpcpc/install
