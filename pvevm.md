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

