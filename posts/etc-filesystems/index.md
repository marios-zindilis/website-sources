---
title: /etc/filesystems
description: Notes on the /etc/filesystems file
first-published: 2014-03-30
---

On RedHat, CentOS, and probably other RPM-based distributions, the 
`/etc/filesystems` file lists the filesystems that are *known* to the 
system, i.e. those filesystems that can be mounted without specifying 
the filesystem type. The contents of that file in a minimal CentOS 
installation are:

    [root@c6min ~]# cat /etc/filesystems
    ext4
    ext3
    ext2
    nodev proc
    nodev devpts
    iso9660
    vfat
    hfs
    hfsplus
