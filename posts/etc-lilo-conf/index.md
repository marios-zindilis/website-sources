---
title: /etc/lilo.conf
description: Notes on LILO configuration file /etc/lilo.conf
first-published: 2014-01-05
---

The LILO bootloader has long been deprecated, in favour of GRUB, 
however it still has an install base on old systems that remain in 
production. The `/etc/lilo.conf` file contains its configuration. After 
any change in that file, it is necessary to reinstall LILO, by simply 
running `lilo`.

Each installed kernel version will have a line in this file, beginning 
with `image=`, followed by the path to the kernel file. For example:

    image=/boot/bzImage-2.6.28
