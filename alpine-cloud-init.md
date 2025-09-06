alpine-Cloud-init.md

# prepare
- [How to create your own cloud-init alpine image for Proxmox](https://twdev.blog/2023/11/alpine_cloudinit/)
- hostname - doesn’t matter, it’ll be changed by cloud-init,
- don’t add any additional users,
- enable ssh,
- disable root ssh login,
```
setup-alpine
hostname: alpine
```
```
apk add \
    util-linux \
    e2fsprogs-extra \
    qemu-guest-agent \
    sudo
```
- Enable qemu-guest-agent service.

```rc-update add qemu-guest-agent```

```
apk add \
    py3-netifaces \ #typo fixed
    cloud-init
```
- Now, configure sudo using ```visudo``` and uncomment rules for ```wheel``` group.

As a last step configure ```/etc/cloud/cloud.cfg```. Specifically ```datasources_list```, remove all sources but ```NoCloud```.

Once you do that, it’s time to disable root password and perform cloud-init-setup. After that there’s no turning back! So, if there’s anything else you want to bake into the image do it now.

```
passwd -d root
setup-cloud-init
poweroff
```
After you power off, don’t start the machine again!

- Add cloud-init
- hardware >> add >> cloud drive
- This will be our cloud-init CDROM. The type of the device (IDE, SCSI) and its number doesn’t matter. cloud-init looks up the drive by the filesystem label which must be set to CIDATA.
- Once you do that, convert the VM to a ```template```.
- Clone to VM
- ```sudo su```
- 

