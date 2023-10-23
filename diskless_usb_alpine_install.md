diskless_usb_alpine_install.md

part 1 : Host
```
setup-alpine
vi /etc/ssh/sshd_config
PermitRootLogin yes
rc-service sshd restart
ip addr

```
part 2 : Client
```
ssh root@<192.168.1.21
apk -U upgrade
fdisk /dev/sdc
modprobe vfat
mkfs.vfat /dev/sdc1

setup-bootable -v /media/cdrom/ /dev/sdc1
reboot

```
ref:

  https://www.fedux.org/articles/setup-alpine-linux-diskless.html


  https://cafayate.net/blog/tech-blog-4/alpine-linux-create-a-bootable-usb-15266

  ## most important

  ```
   Verify that the primary partition is bootable
      p Print list of partitions
      If there is no ‘*’ next to the first partition, follow the next steps:
      a Make the partition bootable (set boot flag)
      1 Partition number 1
  ```
