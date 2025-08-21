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
- PVEvm over Wifi [==>](https://github.com/gogonkt/notes/blob/main/MobileProxmoxWorkstation.md#wifi-setup)
- ```apt install wpasupplicant wireless-tools```
- 
- ```wpa_supplicant -B -i wlp4s0 -c <(wpa_passphrase yourSSID yourpassword)```
- ```dhclient -r wlp4s0```
- ```dhclient wlp4s0```
- 

- ```dhcpcd -k interface```
- ```dhcpcd -n interface```
- The first says to release and deconfigure the interface, and the second says to reload configuration and rebind the interface again.
- 
- ```vi /etc/network/interfaces```
- #1
- ```
        auto wlp4s0
        iface wlp4s0 inet static
        address 192.168.1.108/24
        gateway 192.168.1.1
        wpa-ssid [your wifi ssid]
        wpa-psk [your wifi password]
  ```
- #2
- ```
  auto wlp4s0
  iface wlp4s0 inet dhcp
  wpa-essid MYESSID12345
  wpa-psk MYPASSWORD$1234567
  ```
- 


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

# backup and restore
- [How to Move virtual machine (VM) between different Proxmox VE (PVE) hosts or clusters (General ideas/Methods)](https://dannyda.com/2022/04/26/how-to-move-virtual-machine-vm-between-different-proxmox-ve-pve-hosts-or-clusters-general-ideas-methods/)
- 2.3 Use scp command to transfer the backup to the target PVE host
- ```scp SourceFile user2@10.0.0.2:folder1/DestinationFile```

# Rsync
- [scp stalled while copying large files](https://stackoverflow.com/questions/20625000/scp-stalled-while-copying-large-files)
- ```rsync -avz --progress local/path/some_file usr@server.com:"/some/path/"```
- [How to Use Rsync Command in Linux: 16 Practical Examples](https://www.tecmint.com/rsync-local-remote-file-synchronization-commands/)
- ```rsync -avzh /root/rpmpkgs root@192.168.0.141:/root/```
- [Rsync hangs on large files – Quick fixes !!](https://bobcares.com/blog/rsync-hangs-on-large-files/)

# CPU
- [锐翊6800H-ES小主机PVE Windows11LTSC核显直通记录](https://blog.im.ci/now-life/somethings/1336/)
- [ryzen-gpu-passthrough-proxmox](https://github.com/isc30/ryzen-gpu-passthrough-proxmox)

# Router
- [Setting up a virtual router (pfSense on Proxmox)](https://victoronsoftware.com/posts/setting-up-a-virtual-router/) pfSense
- [How to Set Up Proxmox Network - Virtual Router Configuration for LAN Separation and Access via WireGuard](https://devintrap.com/notes/2024/01/28/how-to-setup-proxmox-network-virtual-router-configuration-for-lan-separation-and-access-via-wireguard/) VyOS
- [Virtual Router in Proxmox with the Skullsaints Onyx](https://kayg.org/updates/virtual-router-proxmox-skullsaints-onyx)
- iptables ebtables nftables  A modern packet filtering framework that replaces both iptables and ebtables. 
- [BridgeNetworkConnections](https://wiki.debian.org/BridgeNetworkConnections) Information about sending packets between multiple physical networks (e.g. a device's WiFi and wired networks) as if they were all connected to each other. For general information about network configuration, see NetworkConfiguration.
- [connecting 2 Linux PCs directly with ethernet cable](https://www.reddit.com/r/linuxquestions/comments/kkkd17/connecting_2_linux_pcs_directly_with_ethernet/)
- [How to use two or more networks at the same time in Linux Mint](https://www.microfusion.org/blog/how-to-use-two-or-more-networks-at-the-same-time-in-linux-mint/)
- [Merging Multiple Internet Connections Into One in Linux](https://www.baeldung.com/linux/merge-several-internet-connections)
- 

- [Proxmox part 6: Virtualized nftables home LAN router](https://blog.rymcg.tech/blog/proxmox/06-router/)
- [微型家用网络折腾记](https://opswill.com/articles/vyos-as-a-home-router.html)
- 

- [Using A Computer With No Internet Connection](https://hagensieker.com/2023/12/27/using-a-computer-with-no-internet-connection/)
- [How to Connect Two Computers Remotely: 4 Easy Methods](https://deskin.io/resource/blog/how-to-connect-two-computers)
- [How to build and run MJPG-Streamer on the Raspberry Pi](https://blog.miguelgrinberg.com/post/how-to-build-and-run-mjpg-streamer-on-the-raspberry-pi)
- [How to build a Linux-based wireless router out of spare parts (1998) (rage.net)](https://news.ycombinator.com/item?id=34666142)

# Linux WiFi Ad-hoc mode
- [Linux WiFi Ad-hoc mode](https://wiki.lm-technologies.com/linux-wifi-ad-hoc-mode/)
- [Ad-hoc networking](https://wiki.archlinux.org/title/Ad-hoc_networking)
```
Warning
This method creates unencrypted ad-hoc network. See #wpa_supplicant for method using WPA encryption.
See Wireless network configuration#iw for a better explanation of the following commands.
Make sure that iw is installed.

Set the operation mode to ibss:

# iw interface set type ibss
Bring the interface up (an additional step like rfkill unblock wifi might be needed):

# ip link set interface up
Now you can create an ad-hoc network. Replace your_ssid with the name of the network and frequency
with the frequency in MHz, depending on which channel you want to use. See the Wikipedia page List
of WLAN channels for a table showing frequencies of individual channels.
# iw interface ibss join your_ssid frequency

```
-
# iw interface ibss join your_ssid frequency

- [Creating wireless ad-hoc network in Linux](https://addisu.taddese.com/blog/creating-wireless-ad-hoc-network-in-linux/)
- ```bash
  # Step 1: Change wifi interface configuration to ad-hoc
  # On both machines:

  iwconfig wlan0 mode Ad-hoc
  iwconfig wlan0 essid MyWifi

  # Step 2: Set the IP addresses
  # On Machine A:

  ifconfig wlan0 192.168.1.1 netmask 255.255.255.0
  #On Machine B:

  ifconfig wlan0 192.168.1.2 netmask 255.255.255.0
  ```
- iw [Replacing iwconfig with iw](https://wireless.docs.kernel.org/en/latest/en/users/documentation/iw/replace-iwconfig.html)

# tty screen resolution
- [Debian and the adventure of the screen resolution](https://ral-arturo.org/2023/01/30/console.html)
- [How to enable 1280x800 resolution in tty?](https://askubuntu.com/questions/17912/how-to-enable-1280x800-resolution-in-tty)
- ```
  #GRUB_GFXMODE=640x480
  GRUB_GFXMODE=1366x768
  GRUB_GFXPAYLOAD_LINUX=keep
  ```
  
# Bluetooth Keyboard
- [Bluetooth keyboard](https://kellner.io/bluetooth-keyboard.html)
- 
