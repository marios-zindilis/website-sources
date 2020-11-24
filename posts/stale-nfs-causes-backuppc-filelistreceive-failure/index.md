--- 
title: Stale NFS Causes BackupPC fileListReceive Failure
description: A failed backup on BackupPC found to be caused by a stale NFS mount on the client 
first-published: 2015-06-18
tags:
- BackupPC
- NFS
---

Recently, one of my BackupPC clients running CentOS failed to backup, with the 
contents of the host log being:

        2015-06-10 01:40:10 incr backup started back to 2015-05-16 08:56:42 (backup #600) for directory /
        2015-06-10 21:40:18 Aborting backup up after signal ALRM
        2015-06-10 21:40:18 Got fatal error during xfer (fileListReceive failed)

...and the last bad XferLOG containing:

        fileListReceive() failed

This happened a couple of times in a row, and the interval between the start 
time of the backup and the failure was consistently 20 hours. While checking, 
I noticed that an `rsync` process started on the client by BackupPC was running 
for about a week. I did an `strace -p <PID>` on the process ID of `rsync` and 
noticed that it was trying to `stat` an old NFS export, mounted from a server 
that no longer exists. 

Although [there are other ways to fix this][1], it was OK for this host to be 
rebooted at the time, problem solved.

<!-- Links -->
[1]: http://joelinoff.com/blog/?p=356 "How to fix stale NFS mounts on linux without rebooting"
