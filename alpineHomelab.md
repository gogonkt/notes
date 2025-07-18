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

## podman: enable cgroups v2 
- https://gitlab.alpinelinux.org/alpine/aports/-/issues/16182

That podman service depends on cgroups. Have you enabled/started the podman service?
```
rc-service podman start
```

Ref.
=============================

https://www.wildtechgarden.ca/doc/server-alpine-linux-docs4web/server-install-config/create-semi-data-install/create-semi-data-install/

# KVM

https://sureshjoshi.com/development/alpine-kvm-host

https://sureshjoshi.com/development/alpine-kvm-config-script
- ==> [alpine-kvm:Alpine-based KVM/QEMU host configuration and virt-install scripts](https://github.com/sureshjoshi/alpine-kvm)

- [KVM Alpine wiki](https://wiki.alpinelinux.org/wiki/KVM)
- [Use Alpine Linux as a Hypervisor (with KVM, QEMU and libvirt) on AMD64 and ARM64](https://lunar.computer/news/kvm-qemu-libvirt-alpine/)

- [Alpine Linux with custom partitions](https://www.alextsang.net/articles/20200921-032859/index.html)

# VM & contianer
- [RunCVM (Run Container VM) is an experimental open-source Docker container runtime, for launching standard container workloads - as well as Systemd, Docker, even OpenWrt - in VMs using 'docker run`](https://github.com/newsnowlabs/runcvm)
- [Incus is a modern, secure and powerful system container and virtual machine manager.](https://github.com/lxc/incus)

# Alpine notes

https://github.com/gogonkt/notes/blob/main/alpine_install.md

https://github.com/gogonkt/notes/blob/main/cagebreak_install.md

https://github.com/gogonkt/notes/blob/main/diskless_usb_alpine_install.md

- Alpine Example /etc/fstab https://gist.github.com/voutilad/ece509d4f6a671132c0b3b5011de4e58
- docker.io banded ! shit! setting socks5 proxy https://askubuntu.com/questions/610333/how-to-set-socks5-proxy-in-the-terminal
```
ssh -D 8080 name@myserver.com


export http_proxy="socks5://127.0.0.1:8080" #proxy
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
- [Windows inside a Docker container.](https://github.com/dockur/windows?tab=readme-ov-file)
- [QEMU in a Docker container.](https://github.com/qemus/qemu)
- [qemu-docker](https://github.com/joshkunz/qemu-docker)
- [container-vm](https://github.com/wy-z/container-vm)

- [How to launch qemu-kvm from inside a Docker container?](https://stackoverflow.com/questions/48422001/how-to-launch-qemu-kvm-from-inside-a-docker-container)
- [Qemu in alpine container](https://github.com/BBVA/kvm/issues/2)
```
podman run --rm -ti --name kvm --cap-add NET_ADMIN -v /media/data/gary-os-v9.0-generic_64.qcow2:/image/image.qcow2 -v /media/data/:/shares --device /dev/kvm:/dev/kvm alpine sh
```
```
apk -U --no-cache add qemu-system-x86_64 qemu bridge-utils dnsmasq bash && rm -rf /var/cache/apk/*
qemu-system-x86_64 -enable-kvm -cpu host -m 1024 -curses -drive file=/image/image.qcow2,format=qcow2,cache=none -usb -usbdevice tablet
```
- install tun # --device=/dev/net/tun
```
apk add qemu-modules
echo tun >> /etc/modules
modprobe tun
```
- run QEMU
```
export http_proxy="socks5://127.0.0.1:8080" #proxy
export https_proxy="socks5://127.0.0.1:8080"
```
- GaryOS Qemu
```
podman run -it --rm --name qemu -v "/media/data/gary-os-v9.0-generic_64.qcow2:/boot.qcow2" -p 8006:8006 --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN -v "/media/data/containers/podman/volumes/_data/qemu/:/storage" --stop-timeout 120 qemux/qemu
```
- ==>Error
```
iptables v1.8.11 (legacy): can't initialize iptables table `nat': Table does not exist (do you need to insmod?)
Perhaps iptables or your kernel needs to be upgraded.
❯ ERROR: The 'ip_tables' kernel module is not loaded. Try this command: sudo modprobe ip_tables iptable_nat
❯ Warning: podman detected, falling back to user-mode networking!
❯ Notice: port mapping will not work without "USER_PORTS" now.
❯ Warning: your configured RAM_SIZE of 4 GB is very close to the 4.7 GB of memory available, please consider a lower value.
❯ Booting image using QEMU v10.0.0...

```
- Alpine Linux Qemu
```
podman run -it --rm --name qemu -e "BOOT=alpine" -p 8006:8006 --device=/dev/kvm --device=/dev/net/tun --cap-add NET_ADMIN -v "/media/data/containers/podman/volumes/_data/qemu/:/storage" --stop-timeout 120 qemux/qemu
```
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
- [Docker container for Firefox](https://github.com/jlesage/docker-firefox)

Launch the Firefox docker container with the following command:

```shell
podman run --net host -e DISPLAY_WIDTH=1366 -e DISPLAY_HEIGHT=768 -d --name=firefox -p 5800:5800 -v /media/data/containers/podman/volumes/_data/:/config:rw jlesage/firefox
```

- [How do I get into a Docker container's shell?](https://stackoverflow.com/questions/30172605/how-do-i-get-into-a-docker-containers-shell)
  ```
  podman  exec -it <mycontainer> sh
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

# Docker X11
- [Docker X11 Client Via SSH](https://dzone.com/articles/docker-x11-client-via-ssh)
```
podman run --net=host --env="DISPLAY" --volume="$HOME/.Xauthority:/root/.Xauthority:rw" xeyes
```
- [Basic container for X11 forwarding goodness](https://gist.github.com/udkyo/c20935c7577c71d634f0090ef6fa8393)
- [x11docker: x11docker logo Run GUI applications in Docker or podman containers.](https://github.com/mviereck/x11docker)
- Desktop [Webtop - Alpine, Ubuntu, Fedora, and Arch based containers containing full desktop environments in officially supported flavors accessible via any modern web browser.](https://github.com/linuxserver/docker-webtop)
# Alpine Linux as AP

- [Creating an Access Point on Raspberry Pi running Alpine Linux](https://gist.github.com/XtendedGreg/5ef72d27c0dd1dbf1f2e2125092e7369)

- [Docker container stack: hostap + dhcp server](https://github.com/shugaoye/docker-rpi-ap)

# Android 
- [budtmo/docker-android](https://github.com/budtmo/docker-android)
- [Youtube: Run Android in a Docker Container!](https://www.youtube.com/watch?v=a1M40roHuRg&ab_channel=TechonFire)
- [redroid (Remote anDroid)](https://github.com/remote-android/redroid-doc)

# [GaryOS](https://github.com/garybgenett/gary-os?tab=readme-ov-file#virtual)
# [EasyOS](https://easyos.org/)

---

# Kubernetes

- [Explaining Kubernetes To My Uber Driver ](https://dev.to/therubberduckiee/explaining-kubernetes-to-my-uber-driver-4f60)

- 
- [Alpine Linux 🌲 K8s in 10 Minutes](https://gist.github.com/gogonkt/1e5e62d0a852d85e1e31a8f3e9ca95c3)
- ==> Alpine wiki [Alpine Linux 🌲 K8s in 10 Minutes](https://wiki.alpinelinux.org/wiki/K8s)
- [Deploy a HA Kubernetes Cluster on Alpine](https://medium.com/@alesson.viana/deploy-a-ha-kubernetes-cluster-on-alpine-6984d96b1743)
- [How to Run a Virtual Machine (VM) on a Local Kubernetes Cluster](https://www.getambassador.io/blog/how-to-run-virtual-machine-vm-local-kubernetes-cluster-guide#prerequisites-setting-up-local-kubernetes-cluster)
- [Installing Kubernetes Homelab Tools for Development and Deploying a Sample Application Using a Jenkins Pipeline](https://homelabdev.com/installing-kubernetes-homelab-tools-for-development-and-deploying-a-application-using-a-jenkins-pipeline/)
- [Kubernetes CheatSheets In A4 ](https://github.com/dennyzhang/cheatsheet-kubernetes-A4)
- [Awesome Kubernetes Resources](https://github.com/tomhuang12/awesome-k8s-resources)

- [Converting an Old MacBook Into an Always-On Personal Kubernetes Cluster](https://devopsdirective.com/posts/2020/03/always-on-minikube/)
- [3 years managing Kubernetes clusters, my 10 lessons.](https://hervekhg.medium.com/3-years-managing-kubernetes-clusters-my-10-lessons-b565a5509f0e)
- [Moving my personal infrastructure to Kubernetes (single-node k3s)](https://stanislas.blog/2025/04/moving-to-k8s/)
- [Getting KubeVirt and K0s to play nicely on Ubuntu 20.04 LTS](https://josebiro.medium.com/getting-kubevirt-and-k0s-to-play-nicely-on-ubuntu-20-04-lts-39c33f18a8b2)
- [Seamless Kubernetes Virtualization: Deploy VMs and Containers with Open Source KubeVirt and k0s](https://www.youtube.com/watch?v=3Vgn37Zvg0E&ab_channel=Mirantis)
- ==>[k0s-kubevirt Tech talk](https://github.com/kevng9/k0s_kubevirt_techtalk)
- [Deploying k0s to alpine](https://blog.devdemand.co/deploying-k0s-alpine/)
- [Deploying k3s on Alpine Linux for Home Automation ](https://stephentanner.com/k3s-on-alpine.html)

- [Alpine and k3s – a lightweight kubernetes experience](https://draghici.net/2020/06/09/alpine-and-k3s-a-lightweight-kubernetes-experience/)
- [Bringing Zero Trust and Observability to VMs in Kubernetes with KubeVirt and Cilium](https://isovalent.com/blog/post/kubevirt-cilium-vm-networking/)

# virtual desktop on Kubernetes
- [How to setup a virtual desktop on Kubernetes in 15 minutes](https://medium.com/devops-dudes/how-to-setup-a-virtual-desktop-on-kubernetes-in-15-minutes-b2d7f213e3e3)
- 

# SSH
- [How to download a file through an SSH server?](https://unix.stackexchange.com/questions/38755/how-to-download-a-file-through-an-ssh-server)
  ```
  one-line solution. Well, how about

  ssh address-of-B 'wget -O - http://server-C/whatever' >> whatever

  i.e. redirection the wget-fetched output to stdout, and redirecting the local output (from ssh running wget remotely) to a file.

  This seems to work, the wget output is just a little confusing ("saved to -"), you can get rid of it by adding -q to the wget   call.
  ```


# [Backlight](https://wiki.archlinux.org/title/Backlight) 
```
echo 1500 > /sys/class/backlight/intel_backlight/brightness
```

# chown 
- [I've mounted my secondary SSD and it wont let me make any files on it. ](https://www.reddit.com/r/archlinux/comments/ouhun6/ive_mounted_my_secondary_ssd_and_it_wont_let_me/)
```
# chown -R your_user /path/to/mount_point
```




# X220 X230
- [ThinkPads#Keymap_Table](https://www.thinkwiki.org/wiki/Install_Classic_Keyboard_on_xx30_Series_ThinkPads#Keymap_Table) 
- [Classic keyboard for Lenovo X230](https://majek.sh/en/classic-keyboard-for-lenovo-x230/)
