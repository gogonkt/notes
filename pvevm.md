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
- 
