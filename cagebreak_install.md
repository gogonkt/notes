# alpine linux cagebreak install

1. setup-alpine
2. vi /etc/ssh/sshd_config
3. ip addr
4. vi /etc/apk/repositories
5. apk -U upgrade
6. apk add -i mesa-dri-gallium seatd dbus cagebreak font-noto firefox
 xterm
7.
```
adduser $USER input
adduser $USER audio
adduser $USER video
adduser $USER seat
setup-devd udev
rc-update add seatd
rc-service seatd start
rc-update add dbus
rc-service dbus start
```

8. $ cat ~/.profile
```
if [ -z $XDG_RUNTIME_DIR ]; then
    export XDG_RUNTIME_DIR="/tmp/$(id -u)-runtime-dir"
    if [ ! -d $XDG_RUNTIME_DIR ]; then
        mkdir $XDG_RUNTIME_DIR
	chmod 0700 $XDG_RUNTIME_DIR
    fi
fi

export XDG_CONFIG_HOME="$HOME/.config"
export XDG_CACHE_HOME="$HOME/.cache"
export XDG_DATA_HOME="$HOME/.local/share"
export XDG_STATE_HOME="$HOME/.local/state"

/usr/libexec/pipewire-launcher > /dev/null 2>&1

if [ -z $WAYLAND_DISPLAY ] && [ $(tty) == /dev/tty1 ]; then
    exec dbus-run-session -- cagebreak
fi
```

ref: 
 https://www.reddit.com/r/AlpineLinux/comments/13o433u/a_simple_alpine_linux_installation_guide_with/
