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

mount /dev/mmcblk0p2 /mnt/  # breack point 1 <== for pick up an installing break
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
010-013
```
vi ~/.profile
export KISS_PATH=''
export KISS_PATH="$HOME/repos/repo/core:$HOME/repos/repo/extra:$HOME/repos/repo/wayland:$KISS_PATH"

# CFLAGS/CXXFLAGS
# NOTE: The 'O' in '-O3' is the letter O and NOT 0 (ZERO). 
export CFLAGS="-O3 -pipe -march=native"
export CXXFLAGS="$CFLAGS"
# MAKEFLAGS
# NOTE: '4' should be changed to match the number of threads.
#       This value can be found by running 'nproc'.
export MAKEFLAGS="-j2"

echo $KISS_PATH

```


ref:

https://kisscommunity.org/kiss/install/

https://kisslinux.org/install.html

https://www.kailashkatheth.com.np/2022/10/kiss-linux-test.html

https://github.com/mcpcpc/install
