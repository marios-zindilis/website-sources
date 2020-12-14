---
title: Set up CentOS 6 as NFS Server
first-published: 2014-07-09
---

These are minimal instructions on how to set up an NFS server running on 
CentOS 6. 

1.  Install the required packages:

        yum install nfs-utils nfs-utils-lib

2.  Enable and start the required services:

        chkconfig rpcbind on 
        chkconfig nfs on
        
        service rpcbind restart
        service nfs restart

3.  Create shares by editing: `/etc/exports`
