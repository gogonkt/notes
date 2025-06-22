talpine_install.md

1. Download Alpine ISO
2. Boot the ISO with Ventoy Flash drive.

3. ****** Lazy Desktop Setup/Fluxbox/Firefox
    ```
    setup-alpine
    setup-xorg-base fluxbox sakura sudo chromium -i
    addgroup mvll input
    addgroup mvll video
    addgroup mvll wheel
    
    # logout switch to user
    startx
    # and then
     fluxbox-generate_menu -t sakura

    apk add font-noto-cjk alsaconf alsa-utils xf86-input-synaptics thunar -i
    alsamixer
    mkdir /etc/X11/xorg.conf.d
    vi /etc/X11/xorg.conf.d/70-synaptics.conf
    
    ```
    70-synaptics.conf https://www.rainingforks.com/blog/2015/how-to-tweak-the-touch-sensitivity-of-a-mouse-touchpad-in-linux.html
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
# .ashrc

https://codeberg.org/dotdmt/AlpineSetup/src/branch/main/Basics

https://news.ycombinator.com/item?id=35837424

 $ENV is for interactive shells, .profile is for login shells. Normal shells source these files according to to which of the two flags are set or not.
```
echo 'ENV="$HOME/.ashrc"' >> $HOME/.profile
```

# Automatice Install

https://wejn.org/2022/04/alpinelinux-unattended-install/

https://git.sr.ht/~bt/alpine-linux-setup  # sway aaa

https://github.com/git-sgmoore/AlpineLinux-DailyDriverDesktop

https://copyprogramming.com/howto/creating-custom-alpine-linux-iso-with-an-answer-file-built-in

https://www.wildtechgarden.ca/deploy-admin/server-alpine-linux-docs4web/server-install-config/create-semi-data-install/

https://www.fedux.org/articles/setup-alpine-linux-diskless.html


https://riedstra.dev/2022/12/alpine-laptop-desktop-how-to

https://www.reddit.com/r/AlpineLinux/comments/13o433u/a_simple_alpine_linux_installation_guide_with/ #sway aaa

https://community.ops.io/jmarhee/setting-up-an-alpine-linux-desktop-workstation-46b8

https://dataswamp.org/~solene/2023-07-14-alpine-linux-from-ram-but-persistent.html


https://gist.github.com/profOnno/e55c1b9be73fa1ff92ec8014b549d01c

http://www.sodface.com/tech/alpine-chromebase-bspwm/

LXQt  https://wiki.alpinelinux.org/wiki/LXQt

sway  https://wiki.archlinux.org/title/User:M0p/Sway_desktop

http://epsi-rns.github.io/desktop/2018/04/03/fluxbox-config.html

# Diskless mode aaa 

https://youtu.be/DM2-5C2SOL0?si=BYllRzOUuA42nnEK

https://wiki.alpinelinux.org/wiki/Alpine_local_backup#Saving_and_loading_ISO_image_customizations

https://wiki.alpinelinux.org/wiki/Create_a_Bootable_Device

Shockingly easy to produce a custom Alpine Linux LiveCD https://eyedeekay.github.io/kloster/

# Sakura

https://github.com/dabisu/sakura

# vis

# tmux

https://gist.github.com/fzero/4338767

# chromium

apk add -i chromium

chromium --proxy-server="http://192.168.1.201:8080"

## socks 5 

https://www.chromium.org/developers/design-documents/network-stack/socks-proxy/

## dark theme 

https://chromewebstore.google.com/detail/google-chrome-%E5%AE%8C%E6%95%B4%E7%9A%84%E9%BB%91%E8%89%B2%E4%B8%BB%E9%A1%8C/ojocmeabgojddapjkfbdbmpeoodhepgd

```
Configuring a SOCKS proxy server in Chrome
To configure chrome to proxy traffic through the SOCKS v5 proxy server myproxy:8080, launch chrome with these two command-line flags:

--proxy-server="socks5://myproxy:8080"

--host-resolver-rules="MAP * ~NOTFOUND , EXCLUDE myproxy"
```

dark https://medium.com/@vipulgote4/how-to-force-dark-mode-on-every-website-in-google-chrome-f52f550a3fd

# youtube + mpv

moccasin boots 24 

https://write.corbpie.com/searching-youtube-videos-with-yt-dlp/

https://www.funkyspacemonkey.com/mpv-youtube-dl-stop-wasting-resources

```
# ~ $ cat ~/.rc

# --ytdl-format= <<     # px yt-dlp -F https://www.youtube.com/watch?v=xY9n3-9YMs8 >> tmpfile  #get the format number
# from  https://github.com/ytdl-org/youtube-dl/blob/master/README.md#format-selection
alias tvb="http_proxy=http://192.168.1.201:8080 mpv --cache=no --profile=720p"
alias tvm="http_proxy=http://192.168.1.201:8080 mpv --cache=no --profile=360p"
alias tv2='http_proxy=http://192.168.1.201:8080 mpv --cache=no --ytdl-format=22 '
alias tv1='http_proxy=http://192.168.1.201:8080 mpv --cache=no --ytdl-format=18 '
```

```
# ~ $ cat ~/.config/mpv/mpv.conf 
[1080p]
ytdl-format=bestvideo[height<=?1080]+bestaudio/best

[720p]
ytdl-format=bestvideo[height<=?720]+bestaudio/best

[480p]
ytdl-format=bestvideo[height<=?480]+bestaudio/best

[360p]
ytdl-format=bestvideo[height<=?360]+bestaudio/best

# Now when you want to play a video, all you have to 
# do is copy the yotube link and in the terminal type 
#      mpv --profile=1080p youtube-link

```
