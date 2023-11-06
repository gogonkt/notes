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
cd ~/repos/repo
git config gpg.ssh.allowedSignersFile .allowed_signers
git config merge.verifySignatures true
```

014-017

```
vi ~/.profile
export KISS_PATH=''
export KISS_PATH="$HOME/repos/repo/core:$HOME/repos/repo/extra:$HOME/repos/repo/wayland:$KISS_PATH"

# CFLAGS/CXXFLAGS
# NOTE: The 'O' in '-O3' is the letter O and NOT 0 (ZERO). 
# export CFLAGS="-O3 -pipe -march=native"
# export CXXFLAGS="$CFLAGS"

# NOTE: from https://codeberg.org/kiss-community/repo/releases
export CFLAGS="-march=x86-64 -mtune=generic -pipe -Os"
export CXXFLAGS="-march=x86-64 -mtune=generic -pipe -Os"

# MAKEFLAGS
# NOTE: '4' should be changed to match the number of threads.
#       This value can be found by running 'nproc'.
export MAKEFLAGS="-j2"

# NOTE: for github
alias proxyon="export http_proxy='http://192.168.1.201:8080';export https_proxy=$http_proxy"
alias proxyoff="unset http_proxy;unset https_proxy"

echo $KISS_PATH # check kiss path

kiss u # update

cd /var/db/kiss/installed && ionice -c3 kiss build *


```

018
```
ls ~/.cache/kiss/sources/linux-headers  
linux-6.1.15.tar.xz
 
mkdir ~/kernel  
cd ~/kernel  
tar xvf  ~/.cache/kiss/sources/linux-headers/linux-*.tar.xz
cd linux-6.1.15
make defconfig

patch -p1 < /usr/share/doc/kiss/wiki/kernel/no-perl.patch

sed '/<stdlib.h>/a #include <linux/stddef.h>' \
   tools/objtool/arch/x86/decode.c > _
mv -f _ tools/objtool/arch/x86/decode.c
kiss b libelf

```

ref:

https://kisscommunity.org/kiss/install/

https://kisslinux.org/install.html

https://www.kailashkatheth.com.np/2022/10/kiss-linux-test.html

https://github.com/mcpcpc/install
