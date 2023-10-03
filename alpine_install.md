alpine_install.md

1. Download Alpine ISO
2. Boot the ISO with Ventoy Flash drive.

3. ****** Lazy Desktop Setup/Fluxbox/Firefox
    ```
    setup-alpine
    setup-xorg-base fluxbox-esr font-terminus alacritty sudo firefox -i
    addgroup mvll input
    addgroup mvll video
    addgroup mvll wheel
    
    # logout switch to user
    startx
    # and then
     fluxbox-generate_menu -t alacritty

    apk add font-noto-cjk alsaconf alsa-utils xf86-input-synaptics thunar -i
    alsamixer
    mkdir /etc/X11/xorg.conf.d
    vi /etc/X11/xorg.conf.d/70-synaptics.conf
    
    ```
   ```
    setup-alpine
    setup-xorg-base fluxbox alacritty font-terminus firefox font-noto-cjk alsaconf alsa-utils xf86-input-synaptics thunar -i
    fluxbox-generate_menu -t alacritty

4. Run script
    ```
    setup-alpine
    
    ip addr
    
    vi /etc/apk/repositories # Repos : 64 JP
    
    apk update && apk upgrade -i #update & upgrade system
    ```
    tools
    ```
    apk add htop lsblk
    ```
    xorg & fluxbox
    https://wiki.alpinelinux.org/wiki/Fluxbox
    https://wiki.alpinelinux.org/wiki/Openbox
    ```
    setup-xorg-base fluxbox alacritty  font-terminus #install xorg and fluxbox
    
    addgroup mvll input &&  addgroup mvll video && addgroup mvll audio
    
    /etc/init.d/udev start
    
    # logout & login
    
    fluxbox-generate_menu -t alacritty
    
    ```
    firefox
    
    proxy: 192.168.1.201:8080

   max video https://addons.mozilla.org/en-US/firefox/addon/maximize-video/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search

    tab-session-manager https://addons.mozilla.org/en-US/firefox/addon/tab-session-manager/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search

   Proxy SwitchyOmega https://addons.mozilla.org/en-US/firefox/addon/switchyomega/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search

   minimal-internet-experience https://addons.mozilla.org/en-US/firefox/addon/minimal-internet-experience/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search

   ```
   https://github.com/gogonkt/notes/raw/main/OmegaOptions.bak
   ```
    ```
    apk add firefox font-noto-cjk
    ```

    mscorefonts
    ```
    apk add msttcorefonts-installer
    update-ms-fonts
    fc-cache -f
    ```
    
   TouchPad
    
    https://wiki.archlinux.org/title/Touchpad_Synaptics
    ```
    apk add sudo xf86-input-synaptics -i
    mkdir /etc/X11/xorg.conf.d
    vi /etc/X11/xorg.conf.d/70-synaptics.conf
    ```
    Alsa
    ```
    apk add alsa-utils alsaconf
    alsamixer
    ```
   File Manager
   ```
   apk add nnn nnn-plugins  udisks2
   nnn-getplugs
   ```

# Automatice Install

https://wejn.org/2022/04/alpinelinux-unattended-install/

https://git.sr.ht/~bt/alpine-linux-setup

https://github.com/git-sgmoore/AlpineLinux-DailyDriverDesktop

https://copyprogramming.com/howto/creating-custom-alpine-linux-iso-with-an-answer-file-built-in
