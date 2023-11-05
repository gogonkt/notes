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

mount /dev/mmcblk0p2 /mnt/  # breack point 1 <==
cd /tmp/
```
001-005
```
ver=23.04.30
url=https://codeberg.org/kiss-community/repo/releases/download/$ver
file=kiss-chroot-$ver.tar.xz
curl -fLO "$url/$file"
sha256sum kiss-chroot-$ver.tar.xz
cd /mnt
tar xvf "$OLDPWD/$file"
```
006-009
```
mount /dev/mmcblk0p1 /mnt/boot # breack point 2 <==
/mnt/bin/kiss-chroot /mnt      # breack point 3 <==

mkdir ~/repos
cd ~/repos
git clone https://codeberg.org/kiss-community/repo
export KISS_PATH="$HOME/repos/repo/core:$HOME/repos/repo/extra:$KISS_PATH"
kiss s \*

```



ref:

https://kisscommunity.org/kiss/install/

https://kisslinux.org/install.html

https://www.kailashkatheth.com.np/2022/10/kiss-linux-test.html

https://github.com/mcpcpc/install
