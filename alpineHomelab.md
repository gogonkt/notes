# Alpine Homelab setting up

## diskless_usb_alpine_install
```
setup-alpine
ip addr

apk -U upgrade
```
in Cfdisk, For exFAT formatted drives using the GPT partitioning scheme, the correct partition type is Microsoft Basic Data Partition

https://wiki.alpinelinux.org/wiki/Create_a_Bootable_Device
```
apk add dosfstools util-linux cfdisk # for GPT

wipefs --all /dev/sda
cfdisk /dev/sdc
mkfs.vfat /dev/sdc1

setup-bootable -v /tmp/ISO /dev/sdc1
reboot

```


https://woozle.org/blog/2023/12-22-alpine-linux-homelab/

  . https://git.woozle.org/neale/toolbox


https://www.wildtechgarden.ca/doc/server-alpine-linux-docs4web/server-install-config/create-semi-data-install/create-semi-data-install/


https://sureshjoshi.com/development/alpine-kvm-host

https://sureshjoshi.com/development/alpine-kvm-config-script

# Alpine notes

https://github.com/gogonkt/notes/blob/main/alpine_install.md

https://github.com/gogonkt/notes/blob/main/cagebreak_install.md

https://github.com/gogonkt/notes/blob/main/diskless_usb_alpine_install.md



