# Alpine Homelab setting up

- diskless_usb_alpine_installu
```
setup-alpine
ip addr

apk -U upgrade
```
- use MBR dos and set boot flag
```
apk add dosfstools util-linux cfdisk # sdc1 no GPT, MBR and EXfat and bootable

wipefs --all /dev/sdc
cfdisk /dev/sdc
mkfs.vfat /dev/sdc1

setup-bootable -v /tmp/ISO /dev/sdc1
reboot

```
- Finishing

https://wiki.alpinelinux.org/wiki/Create_a_Bootable_Device#Finishing_diskless_installation

https://woozle.org/blog/2023/12-22-alpine-linux-homelab/

  . https://git.woozle.org/neale/toolbox

# Podman

https://0ink.net/posts/2025/2025-10-01-podman.html

- install Podman with the following command:
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
modprobe btrfs
echo btrfs >> /etc/modules

mkfs.btrfs -f -L Data /dev/sda5 # -f for avoid ext4 file system exited
lsblk -fl
btrfs filesystem show  #verified
mount /dev/sda5 /media/data
```
- /etc/fstab
```
blkid /dev/sda5
lsblk -f /dev/sda5 # get UUID

UUID=21a7d85f-fe01-43c9-a801-29f5c7ec268b /media/data           btrfs   defaults      0  0

mount -av
df -H
```

## Configuring Podman

- Ensure Podman is not running:
```
service podman stop
```
- Edit /etc/containers/storage.conf, find the [storage] section, and modify the graphroot key to point to your persistent storage location:
```
graphroot = "/media/data/containers/podman"
```
- Make sure that the graphroot directory exists:
```
mkdir -p /media/data/containers/podman
```
- If you stopped Podman earlier, you can start it now:
```
service podman start
```

## Compatibility with Docker
- install
```
apk add docker-cli-compose
```
For convenience, create a small script at /usr/local/bin/podman-compose:
```
#!/bin/sh
# Set DOCKER_HOST pass -H to override
export DOCKER_HOST=unix:///run/podman/podman.sock
exec docker-compose "$@"
```

- Moving of --link'ed containers

Old containers using Container Links can not be moved directly to Podman without changes. The Container Links feature is now obsolete and Docker supports it only for compatibility.

In Podman you are better off creating a new network using:
```
podman create network NETWORK_NAME
```
Connect your containers to that network:
```
podman run -d \
    -p 8080:80 \
    --restart=always \
    --network=NETWORK_NAME \
    nginx:latest
```
Then you can use the container name using the DNS resolver. Note that the default Podman network has this internal DNS resolver disabled and you need to create a new one.

If you are using docker compose a network is created by default and all the containers in the stack will be connected to it.

Ref.
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
- docker.io banded ! shit! setting socks5 proxy https://askubuntu.com/questions/610333/how-to-set-socks5-proxy-in-the-terminal
```
ssh -D 8080 name@myserver.com


export http_proxy="socks5://127.0.0.1:8080"
export https_proxy="socks5://127.0.0.1:8080"

```

- podman-build-from-dockerfile https://devtodevops.com/podman-build-from-dockerfile/



