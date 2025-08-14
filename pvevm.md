pvevm.md

# Proxmox-GitOps
- Proxmox-GitOps implements a self-sufficient, extensible CI/CD environment for provisioning, configuring, and orchestrating Linux Containers (LXC) within Proxmox VE. Leveraging an Infrastructure-as-Code (IaC) approach, it manages the entire container lifecycle—bootstrapping, deployment, configuration, and validation—through version-controlled automation.
- [Proxmox-GitOps](https://github.com/stevius10/Proxmox-GitOps)
- https://github.com/stevius10/Proxmox-GitOps/wiki ==>
- The following commands and configurations are required on Proxmox host:
- ```
  pveam update
  pveam download local debian-12-standard_12.7-1_amd64.tar.zst && pveam update
  ```
- The following commands and configurations are recommended on Proxmox host:
```
# 1. Apply system defaults
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/post-pve-install.sh)"

# 2. Configure container updates
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/cron-update-lxcs.sh)"

# 3. Configure package sources
bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/pve/update-repo.sh)"

# 4. Enable time synchronization
apt install systemd-timesyncd -y && timedatectl set-ntp true
```
- [Proxmox VE CPU Scaling Governor](https://community-scripts.github.io/ProxmoxVE/scripts?id=scaling-governor)


# Ref.
- https://forum.proxmox.com/threads/download-mirrors-for-download-proxmox-com.69400/
- ```
  #212.224.123.70    download.proxmox.com
  #fr.cdn.proxmox.com has address 51.91.38.34
  #fr.cdn.proxmox.com has IPv6 address 2001:41d0:b00:5900::34
  51.91.38.34    download.proxmox.com#212.224.123.70    download.proxmox.com
  #fr.cdn.proxmox.com has address 51.91.38.34
  #fr.cdn.proxmox.com has IPv6 address 2001:41d0:b00:5900::34
  51.91.38.34    download.proxmox.com
  ```
- https://mirrors.tuna.tsinghua.edu.cn/help/proxmox/

# CPU
- [锐翊6800H-ES小主机PVE Windows11LTSC核显直通记录](https://blog.im.ci/now-life/somethings/1336/)
- [ryzen-gpu-passthrough-proxmox](https://github.com/isc30/ryzen-gpu-passthrough-proxmox)

# Router
- [Virtual Router in Proxmox with the Skullsaints Onyx](https://kayg.org/updates/virtual-router-proxmox-skullsaints-onyx)
- iptables ebtables nftables  A modern packet filtering framework that replaces both iptables and ebtables. 
- [BridgeNetworkConnections](https://wiki.debian.org/BridgeNetworkConnections) Information about sending packets between multiple physical networks (e.g. a device's WiFi and wired networks) as if they were all connected to each other. For general information about network configuration, see NetworkConfiguration.
- [connecting 2 Linux PCs directly with ethernet cable](https://www.reddit.com/r/linuxquestions/comments/kkkd17/connecting_2_linux_pcs_directly_with_ethernet/)
- 

- [Proxmox part 6: Virtualized nftables home LAN router](https://blog.rymcg.tech/blog/proxmox/06-router/)
- 

- [Using A Computer With No Internet Connection](https://hagensieker.com/2023/12/27/using-a-computer-with-no-internet-connection/)
- [How to Connect Two Computers Remotely: 4 Easy Methods](https://deskin.io/resource/blog/how-to-connect-two-computers)

# Bluetooth Keyboard
- [Bluetooth keyboard](https://kellner.io/bluetooth-keyboard.html)
- 
