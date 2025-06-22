# Alpine Homelab setting up

## diskless_usb_alpine_install
```
setup-alpine
ip addr

apk -U upgrade
apk add dosfstools util-linux cfdisk # for GPT

cfdisk /dev/sdc
# modprobe vfat
mkfs.vfat /dev/sdc1

setup-bootable -v /media/cdrom/ /dev/sdc1
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



