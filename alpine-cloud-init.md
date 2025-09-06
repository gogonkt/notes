alpine-Cloud-init.md

# prepare
- [How to create your own cloud-init alpine image for Proxmox](https://twdev.blog/2023/11/alpine_cloudinit/)

```
apk add \
    util-linux \
    e2fsprogs-extra \
    qemu-guest-agent \
    sudo
```

```
apk add \
    py3-netifaces \ #typo fixed
    cloud-init
```
- 
