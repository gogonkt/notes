# Alpine Homelab setting up

### diskless_usb_alpine_installu
```
setup-alpine
ip addr

apk -U upgrade
```
### use MBR dos and set boot flag
```
apk add dosfstools util-linux cfdisk # sdc1 no GPT, MBR and EXfat and bootable

wipefs --all /dev/sdc
cfdisk /dev/sdc
mkfs.vfat /dev/sdc1

setup-bootable -v /tmp/ISO /dev/sdc1
reboot

```
### Finishing

https://wiki.alpinelinux.org/wiki/Create_a_Bootable_Device#Finishing_diskless_installation

https://woozle.org/blog/2023/12-22-alpine-linux-homelab/

  . https://git.woozle.org/neale/toolbox

### Podman

https://0ink.net/posts/2025/2025-10-01-podman.html

 install Podman with the following command:
```
apk add podman
```
To enable Podman at startup, use:
```
rc-update add podman
```

### Mount data partition Btrfs

 - Btrfs https://wiki.alpinelinux.org/wiki/Btrfs
 - Btrfs Filesystem /etc/fstab Entry To Mount It  https://www.cyberciti.biz/faq/linux-btrfs-fstab-entry-to-mount-filesystem-at-startup/
```
apk add btrfs-progs

mkfs.btrfs -f -L Data /dev/sda5

```

  
=============================

https://www.wildtechgarden.ca/doc/server-alpine-linux-docs4web/server-install-config/create-semi-data-install/create-semi-data-install/


https://sureshjoshi.com/development/alpine-kvm-host

https://sureshjoshi.com/development/alpine-kvm-config-script

Alpine Linux with custom partitions https://www.alextsang.net/articles/20200921-032859/index.html

# Alpine notes

https://github.com/gogonkt/notes/blob/main/alpine_install.md

https://github.com/gogonkt/notes/blob/main/cagebreak_install.md

https://github.com/gogonkt/notes/blob/main/diskless_usb_alpine_install.md

- Alpine Example /etc/fstab https://gist.github.com/voutilad/ece509d4f6a671132c0b3b5011de4e58



