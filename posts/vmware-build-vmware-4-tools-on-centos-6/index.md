---
title: Building VMware ESXi 4 Tools on CentOS 6
description: How to build VMware tools for a CentOS 6 virtual machine running on an ESXi 4 server
first-published: 2015-04-27
---


CentOS 6 is not directly supported by version 4 of the VMware ESXi hypervisor, 
which means that there are no precompiled binaries for the kernel modules 
required for the VM to talk to the host. This short guide shows you how to 
manually compile the tools. Note that every time that you upgrade to a newer 
kernel version, you will have to do this from scratch -- you will have to 
compile the tools against the new kernel's headers.

Here's the procedure:

1.  From the vSphere client, right click on the machine, open the "Guest" menu 
    and select "Install/Upgrade VMware Tools". This will insert a virtual 
    optical drive in the VM.

2.  Mount the virtual optical drive, *not* directly on the `/mnt` directory, 
    since that will be used later by the installer. For example:

        mkdir /mnt/vmware-tools
        mount -o loop /dev/cdrom /mnt/vmware-tools

3.  Copy the tools in a writeable location (the mount point of the virtual 
    optical drive will be read-only) and decompress it. For example:

        cp /mnt/vmware-tools/VMwareTools-4*.tar.gz /root/
        cd /root/
        tar zxvf VMwareTools-4*.tar.gz

4.  Before starting the installation, install a C compiler and the kernel's 
    source code, since the VMware tools installer will need the kernel headers 
    to include while compiling the modules. For example:

        yum install gcc
        yum install kernel-headers

5.  Next, start the installation. It will ask a lot of questions, on which you 
    can (most probably) accept the default answers.

        cd /root/vmware-tools-distrib
        ./vmware-install.pl

6.  Finally, even though you don't normally need to reboot, do it anyway if you 
    can. That way you will verify that the kernel modules are properly loaded 
    after a reboot. You can check that with:

        [root@centos-6 ~]# lsmod | grep '^vm'
        vmmemctl                8642  0 
        vmware_balloon          7199  0 

7.  If everything went fine, you might want to clean up the files you copied 
    and extracted, although keeping them will speed up the reinstallation of 
    the tools in case you upgrade the kernel to a newer version.
