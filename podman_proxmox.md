grencez.dev

# How to run Podman and Dockge in an Alpine LXC on Proxmox
Date: 2023-11-26

Update: 2025-05-10 (podman-compose cgroups v2 enforcement workaround)

## Motivation
I just want to run containers on Proxmox in a maintainable way.

## How should a solution look?

Must not modify the base Proxmox system.
Run in VM or unprivileged LXC.
Should be lightweight because my system is often tight on RAM.
Run in Alpine Linux LXC.
I want to try something other than Docker managed with Portainer.
Podman managed with Dockge!
Why Dockge? Dockge simplifies the most tedious parts of containers: creation, execution, and navigation. I also appreciate how it exposes all the most important info without extra clicks. It’s a very new project, so I don’t know what else to expect, but I also don’t mind running an occasional podman command to do other stuff.

Why not Yacht? Yacht did seem to work with Docker, but its UI moves around too much for me. Its disappearing menus and scrollbars make me lose focus.

Why not Portainer? Portainer introduced a distracting advertisement for its Business Edition earlier this year. It became less obnoxious after 6 months, but it’s clear that the project cannot exist as purely open source.

Why not Kubernetes? I’d like to, but Minikube and K3s complain about Cgroups.

Why not in a VM? If RAM wasn’t a concern, I would have used a virtual machine like the Proxmox admin guide recommends for application containers. Though I must admit that LXCs and ZFS filesystems are more accessible than VMs and ZFS volumes.

## Install
Before continuing, I should note a few non-standard decisions with my setup:

Podman uses fuse-overlayfs because it works and also fits my mental model.
Dockge looks for compose.yaml files in /opt/compose/ instead of /opt/stacks/ because plural directory names are silly.
Dockge is configured in /opt/compose/dockge/compose.yaml instead of /opt/dockge/compose.yaml.
Dockge stores data in a Podman-managed dockge_data volume rather than a /opt/dockge/data/ directory.

## LXC
In Proxmox, download an Alpine “CT Template” and then “Create CT” with it. There’s nothing particularly special in the creation process.

Add fuse feature in the Proxmox CT’s options.
(optional) Mount a few drives. I do this to keep data organized in its underlying ZFS storage.
/opt/compose (small, holds compose.yaml files)
/var/lib/containers (holds images, use lightweight snapshot/backup policy)
/var/lib/containers/storage/volumes (holds container data)

## Podman
Boot into the new Alpine LXC and run the following commands.

```
# Testing repository is required for podman-compose.
echo 'https://dl-cdn.alpinelinux.org/alpine/edge/testing' >> /etc/apk/repositories
apk update
apk upgrade
apk add fuse-overlayfs podman-docker podman-compose
# Ensure Podman uses fuse-overlayfs.
sed -i -e 's:.*mount_program *=.*:mount_program = "/usr/bin/fuse-overlayfs":' /etc/containers/storage.conf
# Always start Podman on boot.
rc-update add podman boot
# We updated a lot. Get a fresh boot.
reboot
```

## Dockge
After rebooting into the Alpine LXC, create a Dockge compose.yaml file and run it with the following commands. It differs from the default compose.yaml in the ways mentioned earlier.

```
mkdir -p /opt/compose/dockge

cat >/opt/compose/dockge/compose.yaml <<HERE
x-podman:
  in_pod: false
volumes:
  dockge_data:
    driver: local
    name: "dockge_data"
services:
  dockge:
    image: louislam/dockge:1
    restart: always
    ports:
      # Host Port : Container Port
      - 5001:5001
    volumes:
      - /var/run/podman/podman.sock:/var/run/docker.sock
      - dockge_data:/app/data
      - /opt/compose:/opt/compose
    environment:
      - DOCKGE_STACKS_DIR=/opt/compose
HERE

cd /opt/compose/dockge
podman-compose up -d
```

The in_pod: false part is to work around newer podman-compose versions enforcing cgroups v2. Kudos to wrobelda on Proxmox forums for this tip.

## Test
That’s it. Dockge should now be accessible on port 5001. If you want a simple test, use the following YAML file to set up a stack. It just prints a version number and exits.

```
# This file is /opt/compose/fildesh_oneoff/compose.yaml
# if you name the stack "fildesh_oneoff".
services:
  fildesh:
    restart: no
    image: ghcr.io/fildesh/fildesh:latest
    command: --version
```

This site is open source. Improve this page.
