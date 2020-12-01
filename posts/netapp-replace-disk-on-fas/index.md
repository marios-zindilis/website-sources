---
title: Replace Failed Disk on NetApp FAS
Description: How to replace a failed disk on a NetApp FAS
first-published: 2015-03-30
---

After physically replacing the disk, the new disk might not be assigned to the 
controller on which the old disk was assigned, depending on the value of the 
`disk.auto_assign` option, which you can check with:

    options disk.auto_assign

Even if the value of that option is `on`, the disk might still remain 
unassigned, in which case you will see a message for unassigned disks in the 
end of the output of the command:

    disk show

You can see which disks are unassinged with

    disk show -n

To assign a disk to a controller, SSH to that controller and do:

    disk assign XX.YY.ZZ

... where `XX.YY.ZZ` is the name of the disk, as obtained by `disk show -n`.
Example output:

    FAS> disk assign 01.23.45
    Fri May 13 00:00:02 [FAS:diskown.changingOwner:info]: changing ownership for disk 12.34.45 (S/N ABCDEF) from unowned (ID 1234567890) to FAS (ID 0987654321)  
