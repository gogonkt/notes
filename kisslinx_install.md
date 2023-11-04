# kisslinx_install.md

Pre-
@root
```
fdisk /dev/mmcblk0
  P1 +500M ef fat32 boot
  P2 +20G ext4
modprobe vfat
mkfs.vfat -F 32 /dev/mmcblk0p1
mkfs.ext4 /dev/mmcblk0p2

mount /dev/mmcblk0p2 /mnt/
cd /tmp/
```
001-005
```
ver=23.04.30
url=https://codeberg.org/kiss-community/repo/releases/download/$ver
file=kiss-chroot-$ver.tar.xz
curl -fLO "$url/$file"
sha256sum kiss-chroot-$ver.tar.xz
```




ref:
https://kisscommunity.org/kiss/install/
https://kisslinux.org/install.html
https://github.com/mcpcpc/install
