---
title: Convert MBR Partition Table to GPT on CentOS 6
description: How to use gdisk to convert MBR to GPT on CentOS 6
first-published: 2013-11-06
last-modified: 2013-11-23
---

To have partitions larger than 2 TeraBytes, you need to create a GPT disk, as opposed to an MBR one. This, however, is
not possible during the installation of CentOS, so you might need to handle it either before the installation (e.g:
with booting a Live CD that supports GPT and creating the partition table before the installation) or after the
installation, with the use of `gdisk`.

Using `gdisk` is actually very easy, although you should keep in mind that there is no guarrantee that your data will
be safe.

For this example, installation of CentOS 6.2 was done on a system with one 3 TB disk. During the installation, the
following partitions were created:

```
/dev/sda1  0.5 GB /boot
/dev/sda2 13.0 GB LVM PV (4GB swap, 9GB /root)
```

This left almost all of the 3 TB on the disk available.

After the installation is done, download `gdisk` for CentOS 6 from OpenSUSE Build Service. You can also find more
versions of `gdisk` at [rodsbooks.com](http://www.rodsbooks.com/gdisk/download.html). Example download:

    wget http://download.opensuse.org/repositories/home:/srs5694/CentOS_CentOS-6/x86_64/gdisk-0.8.8-133.1.x86_64.rpm

After the download completes, install with:

    yum install gdisk-0.8.8-133.1.x86_64.rpm

You can now very simply convert the MBR partition table to GPT with:

    gdisk /dev/sda

The new GPT will be available on the next reboot. You can then create 
a partition bigger than 2 TB in the unallocated space, either with 
`gdisk` or with `parted`, then create a filesystem inside it with `mkfs`, 
and finally mount it and start using it! 

See Also
--------

*   [gdisk WebSite](http://www.rodsbooks.com/gdisk/)
