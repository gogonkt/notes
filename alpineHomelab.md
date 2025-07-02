# Alpine Homelab setting up

- diskless_usb_alpine_install
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
- A Slim Home Server with Alpine Linux https://willhbr.net/2025/03/09/a-slim-home-server-with-alpine-linux/
- == >Configure Rootless Podman Remote on Alpine Linux https://willhbr.net/2025/01/18/configure-rootless-podman-remote-on-alpine-linux/
- ==> Rebuilding My Personal Infrastructure With Alpine Linux and Docker https://www.wezm.net/technical/2019/02/alpine-linux-docker-infrastructure/
- podman-build-from-dockerfile https://devtodevops.com/podman-build-from-dockerfile/
- alpine stable base + up-to-date packages model https://www.wezm.net/technical/2019/02/alpine-linux-docker-infrastructure/
- ==> but it's not as simple as Alpine Linux makes it. With Alpine, you just add the edge repository as a @edge alias in /etc/apk/repositories and then install package-name@edge @ Why Alpine Linux is my new favourite distro https://www.reddit.com/r/linux/comments/1g6t8wg/why_alpine_linux_is_my_new_favourite_distro/
- ipv6 https://gist.github.com/arvati/8f59e3b03458567424efa42174992705
```
modprobe ipv6
echo "ipv6" >> ./etc/modules
```
- If they were all inside your home directory, sane default permissions would be 755 for directories and 644 for files. This is a good job for find. https://www.reddit.com/r/linuxquestions/comments/11o8t5j/how_to_restore_sane_default_permissions_after/
```
# find all directories in [directory] and change permissions to 755
find [directory] -type d -exec chmod 755 {} \;

# find all files in [directory] and change permissions to 644
find [directory] -type f -exec chmod 644 {} \;
```

Dockers
===
- Windows inside a Docker container.  https://github.com/dockur/windows?tab=readme-ov-file
- Docker image for running MetaTrader 4 or MetaTrader 5 on Linux with X11 forwarding. https://hub.docker.com/r/mintjetos/metatrader

-- optional:
```
apk add xhost
apk add iptables
mkdir -p /media/data/containers/podman/volumes/_data/mt4
mkdir -p /tmp/.Xauthority

```
-- copy mt4 to mt4 volume
```
mkdir -p /media/sdcard
mount -t ntfs-3g /dev/mmcblk0p1 /media/sdcard
cp -r /media/sdcard/XM\ Global\ MT4.portable/* ./volumes/_data/mt4
chown -R mvll:mvll ./volumes/_data/mt4/*
ls -lha ./volumes/_data/mt4/

```
-- fireup mt4 container
```
podman run -d -it --name mt4 -e VERSION=mt4 -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v /tmp/.Xauthority:/tmp/.Xauthority -v /media/data/containers/podman/volumes/_data/mt4:'/home/mint/.mt4/drive_c/Program Files (x86)/MetaTrader 4' mintjetos/metatrader:latest

docker start mt4
```
-- use:
```
xhost +
mkdir -p /tmp/.Xauthority
docker start mt4
docker stop mt4
```
- docker-wine https://github.com/scottyhardy/docker-wine
- Dockerized Arduino IDE https://github.com/tombenke/darduino
- ==> [Running GUI apps with Docker](https://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/)
- [Running desktop applications inside a docker container](https://github.com/nggit/dorun)
- [Run GUI applications and desktops in docker and podman containers. Focus on security.](https://github.com/mviereck/x11docker)
- [huntprod/asm](https://github.com/huntprod/docker-asm)
- [The Moby Project](https://github.com/moby/moby)
- [Assembly & Reverse Engineering](https://github.com/Catherine22/Assembly)
- [Programming in assembly language tutorial](https://github.com/mschwartz/assembly-tutorial)
- [The ASM Rosetta Stone](https://github.com/lowleveltv/rosetta-stone)


# Alpine Linux as AP

- [Creating an Access Point on Raspberry Pi running Alpine Linux](https://gist.github.com/XtendedGreg/5ef72d27c0dd1dbf1f2e2125092e7369)

- [Docker container stack: hostap + dhcp server](https://github.com/shugaoye/docker-rpi-ap)

# Android 
- [budtmo/docker-android](https://github.com/budtmo/docker-android)
- [Youtube: Run Android in a Docker Container!](https://www.youtube.com/watch?v=a1M40roHuRg&ab_channel=TechonFire)
- [redroid (Remote anDroid)](https://github.com/remote-android/redroid-doc)


---

# Kubernetes

- [Explaining Kubernetes To My Uber Driver ](https://dev.to/therubberduckiee/explaining-kubernetes-to-my-uber-driver-4f60)

- 
- [Alpine Linux 🌲 K8s in 10 Minutes](https://gist.github.com/gogonkt/1e5e62d0a852d85e1e31a8f3e9ca95c3)
- [Deploy a HA Kubernetes Cluster on Alpine](https://medium.com/@alesson.viana/deploy-a-ha-kubernetes-cluster-on-alpine-6984d96b1743)
- [How to Run a Virtual Machine (VM) on a Local Kubernetes Cluster](https://www.getambassador.io/blog/how-to-run-virtual-machine-vm-local-kubernetes-cluster-guide#prerequisites-setting-up-local-kubernetes-cluster)
- [Installing Kubernetes Homelab Tools for Development and Deploying a Sample Application Using a Jenkins Pipeline](https://homelabdev.com/installing-kubernetes-homelab-tools-for-development-and-deploying-a-application-using-a-jenkins-pipeline/)
- [Kubernetes CheatSheets In A4 ](https://github.com/dennyzhang/cheatsheet-kubernetes-A4)
- [Awesome Kubernetes Resources](https://github.com/tomhuang12/awesome-k8s-resources)

- [Converting an Old MacBook Into an Always-On Personal Kubernetes Cluster](https://devopsdirective.com/posts/2020/03/always-on-minikube/)
