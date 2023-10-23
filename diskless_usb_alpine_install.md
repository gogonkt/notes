diskless_usb_alpine_install.md

ref:

  https://www.fedux.org/articles/setup-alpine-linux-diskless.html


  https://cafayate.net/blog/tech-blog-4/alpine-linux-create-a-bootable-usb-15266

  ## most important

 Verify that the primary partition is bootable
    p Print list of partitions
    If there is no ‘*’ next to the first partition, follow the next steps:
    a Make the partition bootable (set boot flag)
    1 Partition number 1
