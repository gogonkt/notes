internet-sharing.md

# internet-sharing.md
- [How to Share Wired Internet Via Wi-Fi and Vice Versa on Linux](https://www.tecmint.com/share-internet-in-linux/)
- [internet-sharing.sh](https://gist.github.com/EnigmaCurry/34fd778ad8108b2212a4e0547a51fe5c)
- [arch-wlan-to-eth.sh](https://gist.github.com/dimkouv/8aa8e509f562b103fe20a51cffc1af1c)
- ## AP
- [Set up an access point on Linux using hostapd and dnsmasq](https://finchsec-1672417305892.hashnode.dev/linux-ap-hostapd-dnsmasq-dhcp)

- OpenWrt
- [Proxmox PVE上安裝Openwrt的幾個重點：VM or LXC？哪個鏡像？網卡直通？一鍵安裝？](https://upsangel.com/openwrt/how-to-install-openwrt-on-proxmox/)
- [Run an OpenWRT VM on Proxmox VE](https://i12bretro.github.io/tutorials/0405.html)
- [setup_openwrt_lxc_container_proxmox.sh](https://gist.github.com/suuhm/053f819b000bee4af922d66ff6c5d32e)
- [install_openwrt_proxmox.sh](https://gist.github.com/jaminmc/7e786a8947746439f7b8a8e2726e629d)

# Wall
- [singbox 软网关篇](https://www.byxiao.top/archives/singboxForLinux)
- [singbox搭配dae实现软网关！透明代理！比openwrt还好用！效率更高！](https://www.youtube.com/watch?v=OpcDct3cqPI&t=743s)
- Linux 安装 Tailscale 并设置出口节点 keyword
- [极简singbox配置](https://linux.do/t/topic/649153)

# Tailscale
- [Setting up an exit node for LAN resource access with Docker compose](https://mazmr.com/posts/tailscale_exit-node_lan-access/)
- [Run tailscale in docker with exit node enabled](https://dausruddin.com/run-tailscale-in-docker-with-exit-node-enabled/)
- [How to install Tailscale on Proxmox, not a CT or VM](https://www.reddit.com/r/Proxmox/comments/17rpsgz/how_to_install_tailscale_on_proxmox_not_a_ct_or_vm/)
- ```
  I run tailscale in an Ubuntu 22.04 lxc on proxmox. Follow these steps to get it working in an unprivileged lxc.
  https://tailscale.com/kb/1130/lxc-unprivileged/ Then you can use the regular Ubuntu install instructions
  https://tailscale.com/kb/1031/install-linux/ Then I have it setup as a subject router to allow me to access anything on
   my local network https://tailscale.com/kb/1019/subnets/?tab=linux And also as an exit node in case I want to use it as
   a VPN to route all my traffic through my home network from away https://tailscale.com/kb/1103/exit-nodes/?tab=linux
  All in all it takes like 5 minutes to get everything set up and running.
  ```
