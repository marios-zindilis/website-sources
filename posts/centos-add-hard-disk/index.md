---
title: Add a hard disk on a CentOS 6 system
description: How to add an new hard disk on an existing CentOS 6 system
first-published: 2013-11-02
last-modified: 2015-07-25
---

<p class="lead">
  This document shows how to add a 50GB hard disk on a CentOS 6 system, 
  prepare it for usage with 
  <abbr title='Logical Volume Management'>LVM</abbr>, and mount it as the MySQL 
  data directory. This was tested on a VMware Player virtual machine, with 
  CentOS 6.4, and should be safe to run on any 6.x CentOS version.
</p>

## Addition of new hard disk ##

The new disk was added on the CentOS 6.4 machine in VMware Player. The 
operating system doesn't see it yet, this is the previous status:

    [root@c6 ~]# df -h
    Filesystem            Size  Used Avail Use% Mounted on
    /dev/mapper/vg_c6-lv_root
                           18G  1.3G   16G   8% /
    tmpfs                 947M     0  947M   0% /dev/shm
    /dev/sda1             194M   89M   96M  49% /boot
    
    [root@c6 ~]# pvs
      PV         VG    Fmt  Attr PSize  PFree
      /dev/sda2  vg_c6 lvm2 a--  19.80g    0 
    
    [root@c6 ~]# vgs
      VG    #PV #LV #SN Attr   VSize  VFree
      vg_c6   1   2   0 wz--n- 19.80g    0 
    
    [root@c6 ~]# lvs
      LV      VG    Attr      LSize  Pool Origin Data%  Move Log Cpy%Sync Convert
      lv_root vg_c6 -wi-ao--- 17.80g
      lv_swap vg_c6 -wi-ao---  2.00g

`fdisk` sees the new `/dev/sdb` disk:

    [root@c6 ~]# fdisk -l
    
    Disk /dev/sda: 21.5 GB, 21474836480 bytes
    255 heads, 63 sectors/track, 2610 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00072251
    
       Device Boot      Start         End      Blocks   Id  System
    /dev/sda1   *           1          26      204800   83  Linux
    Partition 1 does not end on cylinder boundary.
    /dev/sda2              26        2611    20765696   8e  Linux LVM
    
    Disk /dev/sdb: 53.7 GB, 53687091200 bytes
    255 heads, 63 sectors/track, 6527 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000


    Disk /dev/mapper/vg_c6-lv_root: 19.1 GB, 19113443328 bytes
    255 heads, 63 sectors/track, 2323 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000
    
    
    Disk /dev/mapper/vg_c6-lv_swap: 2147 MB, 2147483648 bytes
    255 heads, 63 sectors/track, 261 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x00000000
    
Create a partition table
------------------------

The first step to make this disk usable by the system, is to create a 
partition table on it, with `fdisk`:

    [root@c6 ~]# fdisk /dev/sdb
    
Print the current partition table with `p`. At the moment, there are no 
partitions, so the output is empty:

    Command (m for help): p
    
    Disk /dev/sdb: 53.7 GB, 53687091200 bytes
    255 heads, 63 sectors/track, 6527 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x1fdf1a3e
    
       Device Boot      Start         End      Blocks   Id  System
    
Create a new primary partition, with `n`, and allow it to occupy the 
entire drive space:

    Command (m for help): n
    Command action
       e   extended
       p   primary partition (1-4)
    p
    Partition number (1-4): 1
    First cylinder (1-6527, default 1): 
    Using default value 1
    Last cylinder, +cylinders or +size{K,M,G} (1-6527, default 6527): 
    Using default value 6527

Print the partition table again to verify:

    Command (m for help): p
    
    Disk /dev/sdb: 53.7 GB, 53687091200 bytes
    255 heads, 63 sectors/track, 6527 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x1fdf1a3e
    
       Device Boot      Start         End      Blocks   Id  System
    /dev/sdb1               1        6527    52428096   83  Linux

In the above output, the type of the partition is `Linux`, which is not 
what we need. With `t`, set the type of the partition to `8e`, i.e. 
Linux LVM:

    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): 8e
    Changed system type of partition 1 to 8e (Linux LVM)

Print the partition table once more, to verify:

    Command (m for help): p 
    
    Disk /dev/sdb: 53.7 GB, 53687091200 bytes
    255 heads, 63 sectors/track, 6527 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x1fdf1a3e
    
       Device Boot      Start         End      Blocks   Id  System
    /dev/sdb1               1        6527    52428096   8e  Linux LVM

That's more like it! Now save the new partition table with `w`:

    Command (m for help): w
    The partition table has been altered!
    
    Calling ioctl() to re-read partition table.
    Syncing disks.

Partial output of `fdisk -l` now shows that the partition is available 
to the operating system for further handling:

    [root@c6 ~]# fdisk -l
    Disk /dev/sdb: 53.7 GB, 53687091200 bytes
    255 heads, 63 sectors/track, 6527 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x1fdf1a3e
    
       Device Boot      Start         End      Blocks   Id  System
    /dev/sdb1               1        6527    52428096   8e  Linux LVM

Create LVM Physical Volume, Volume Group and Logical Volume
-----------------------------------------------------------

Next step is to create a new LVM Physical Volume:

    [root@c6 ~]# pvcreate /dev/sdb1
      Physical volume "/dev/sdb1" successfully created

The new PV now appears in `pvs` output:

    [root@c6 ~]# pvs
      PV         VG    Fmt  Attr PSize  PFree 
      /dev/sda2  vg_c6 lvm2 a--  19.80g     0 
      /dev/sdb1        lvm2 a--  50.00g 50.00g

`pvdisplay` will now list the new physical volume, but it will be noted 
as non-allocatable, in other words non usable yet, because it contains 
no Volume Group:

    [root@c6 ~]# pvdisplay
      --- Physical volume ---
      PV Name               /dev/sda2
      VG Name               vg_c6
      PV Size               19.80 GiB / not usable 3.00 MiB
      Allocatable           yes (but full)
      PE Size               4.00 MiB
      Total PE              5069
      Free PE               0
      Allocated PE          5069
      PV UUID               LqOFBg-4lUi-Uen7-2vtO-EFkf-Cp8p-XvLiHX
       
      "/dev/sdb1" is a new physical volume of "50.00 GiB"
      --- NEW Physical volume ---
      PV Name               /dev/sdb1
      VG Name               
      PV Size               50.00 GiB
      Allocatable           NO
      PE Size               0   
      Total PE              0
      Free PE               0
      Allocated PE          0
      PV UUID               6Nv0Xf-72ao-Nfph-TfvN-26FS-ssE8-QrEsXx

Create a new LVM Volume Group with `vgcreate`. In this example, the 
Volume Group name is `vg_mysql`, because it's intended for use as the 
data directory of a MySQL server, but it can be anything.

    [root@c6 ~]# vgcreate vg_mysql /dev/sdb1
      Volume group "vg_mysql" successfully created

The new VG will now show up in the output of `vgs`:

    [root@c6 ~]# vgs
      VG       #PV #LV #SN Attr   VSize  VFree 
      vg_c6      1   2   0 wz--n- 19.80g     0 
      vg_mysql   1   0   0 wz--n- 50.00g 50.00g

Now the output of `pvdisplay` will change, to show that the Physical 
Volume is actually usable:

    [root@c6 ~]# pvdisplay
      --- Physical volume ---
      PV Name               /dev/sdb1
      VG Name               vg_mysql
      PV Size               50.00 GiB / not usable 3.31 MiB
      Allocatable           yes 
      PE Size               4.00 MiB
      Total PE              12799
      Free PE               12799
      Allocated PE          0
      PV UUID               6Nv0Xf-72ao-Nfph-TfvN-26FS-ssE8-QrEsXx
       
      --- Physical volume ---
      PV Name               /dev/sda2
      VG Name               vg_c6
      PV Size               19.80 GiB / not usable 3.00 MiB
      Allocatable           yes (but full)
      PE Size               4.00 MiB
      Total PE              5069
      Free PE               0
      Allocated PE          5069
      PV UUID               LqOFBg-4lUi-Uen7-2vtO-EFkf-Cp8p-XvLiHX

Now that a Volume Group was created inside the Physical Volume, none of 
the space on the Physical Volume is free any more, as shown in the 
output of `pvdisplay` above, but all of the space in the Volume Group is 
free, as shown by `vgdisplay`:

    [root@c6 ~]# vgdisplay
      --- Volume group ---
      VG Name               vg_mysql
      System ID             
      Format                lvm2
      Metadata Areas        1
      Metadata Sequence No  1
      VG Access             read/write
      VG Status             resizable
      MAX LV                0
      Cur LV                0
      Open LV               0
      Max PV                0
      Cur PV                1
      Act PV                1
      VG Size               50.00 GiB
      PE Size               4.00 MiB
      Total PE              12799
      Alloc PE / Size       0 / 0   
      Free  PE / Size       12799 / 50.00 GiB
      VG UUID               cwEyOs-rIqq-hc5J-cl45-kSZ0-i4dV-HiGE2l
       
      --- Volume group ---
      VG Name               vg_c6
      System ID             
      Format                lvm2
      Metadata Areas        1
      Metadata Sequence No  3
      VG Access             read/write
      VG Status             resizable
      MAX LV                0
      Cur LV                2
      Open LV               2
      Max PV                0
      Cur PV                1
      Act PV                1
      VG Size               19.80 GiB
      PE Size               4.00 MiB
      Total PE              5069
      Alloc PE / Size       5069 / 19.80 GiB
      Free  PE / Size       0 / 0   
      VG UUID               pp0134-mUkA-x3jp-FeVr-0ixo-Lxa4-eX1PCx

Next, create an LVM Logical Volume with `lvcreate`:

    [root@c6 ~]# lvcreate -L 51196M -n lv_mysql vg_mysql
      Logical volume "lv_mysql" created

In the above command, the size 51196M is the number of Physical Extends 
(12799) multiplied by the size of the Physical Extend (4.0 MBytes). This 
will cause the Logical Volume to occupy all of the Volume Group. Verify 
with `lvs`:

    [root@c6 ~]# lvs
      LV       VG       Attr      LSize  Pool Origin Data%  Move Log Cpy%Sync Convert
      lv_root  vg_c6    -wi-ao--- 17.80g                                             
      lv_swap  vg_c6    -wi-ao---  2.00g                                             
      lv_mysql vg_mysql -wi-a---- 50.00g

Creating the filesystem
-----------------------

The next step is to create a filesystem inside the LVM Logical Volume. 
The choice of filesystem depends on a multitude of factors, but for 
this example let's stick with `ext4`:

    [root@c6 ~]# mkfs.ext4 /dev/vg_mysql/lv_mysql 
    mke2fs 1.41.12 (17-May-2010)
    Filesystem label=
    OS type: Linux
    Block size=4096 (log=2)
    Fragment size=4096 (log=2)
    Stride=0 blocks, Stripe width=0 blocks
    3276800 inodes, 13106176 blocks
    655308 blocks (5.00%) reserved for the super user
    First data block=0
    Maximum filesystem blocks=0
    400 block groups
    32768 blocks per group, 32768 fragments per group
    8192 inodes per group
    Superblock backups stored on blocks: 
    	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
    	4096000, 7962624, 11239424
    
    Writing inode tables: done                            
    Creating journal (32768 blocks): done
    Writing superblocks and filesystem accounting information: done
    
    This filesystem will be automatically checked every 39 mounts or
    180 days, whichever comes first.  Use tune2fs -c or -i to override.

Mounting the filesystem
-----------------------

The last step in this procedure, is to actually mount the brand new 
filesystem at a mountpoint inside our filesystem hierarchy. 

    [root@c6 ~]# mkdir /mnt/test
    [root@c6 ~]# mount /dev/vg_mysql/lv_mysql /mnt/test
    [root@c6 ~]# mount | grep mysql
    /dev/mapper/vg_mysql-lv_mysql on /mnt/test type ext4 (rw)

This verifies that our filesystem has been created, and can be mounted.

Setting up MySQL in the new Logical Volume
------------------------------------------

Very briefly, what you need to do to set up MySQL's data directory to 
be the new Logical Volume is:

1.  Install, start, enable and automatically configure the MySQL Server:

        [root@c6 ~]# yum -y install mysql-server
        [root@c6 ~]# service mysqld start
        [root@c6 ~]# chkconfig mysqld on
        [root@c6 ~]# mysql_secure_installation

2.  Stop MySQL, back the original data directory up, mount the new one, 
    and copy all the data over:

        [root@c6 lib]# service mysqld stop
        [root@c6 lib]# cd /var/lib
        [root@c6 lib]# mv mysql mysql-original
        [root@c6 lib]# mkdir mysql
        [root@c6 lib]# mount /dev/vg_mysql/lv_mysql /var/lib/mysql
        [root@c6 lib]# chown mysql:mysql mysql
        [root@c6 lib]# cp -Rpv mysql-original/* mysql/
        [root@c6 lib]# rm -rf mysql/lost+found/

3.  Finally, start the MySQL server again:

        [root@c6 lib]# service mysqld start

That's it. You can now see that the new LVM Logical Volume is mounted, 
and is being used for MySQL:

    [root@c6 lib]# df -h
    Filesystem            Size  Used Avail Use% Mounted on
    /dev/mapper/vg_c6-lv_root
                           18G   17G   22M 100% /
    tmpfs                 947M     0  947M   0% /dev/shm
    /dev/sda1             194M   89M   96M  48% /boot
    /dev/mapper/vg_mysql-lv_mysql
                           50G  201M   47G   1% /var/lib/mysql
