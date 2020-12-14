---
title: Restart entire NFS Stack
description: How to restart the entire NFS stack
first-published: 2013-11-13
---

    service nfs stop
    service nfslock stop
    service portmap stop
    service portmap start
    service nfslock start
    service nfs start
