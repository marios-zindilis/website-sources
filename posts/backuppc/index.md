---
title: BackupPC
Description: Notes and links on BackupPC
first-published: 2014-12-18
last-modified: 2015-06-18
---

Random tips for BackupPC:

1.  On your client systems (those that will be backed up by BackupPC), rotate 
    your logs (whether compressed or not) with dates in the filenames, instead 
    of appending prefixes such as `.1`, `.2`, `.3`, etc. The benefit from this 
    is that BackupPC will ignore old logs on new runs, since they will have the 
    same name and the same checksum. If you rotate logs with numbered names, 
    BackupPC will transfer them again, since the name will have changed. This 
    configuration is achieved with the `dateext` parameter set in `logrotate` 
    configuration file, which on CentOS 6 is at `/etc/logrotate.conf`, by 
    default.

2.  If `mlocate` is installed on the BackupPC system, you should exclude the 
    backup directory from being indexed by the nightly run of `updatedb`, 
    otherwise `/var/lib/mlocate/mlocate.db` will become enormous. To exclude 
    the backup directory, edit `/etc/updatedb.conf` and append the directory 
    path to the end of the line for the `PRUNEPATHS` variable.

See Also
--------

*   **BackupPC Archive** is a Perl script that maintains archive copies of 
    backups taken with BackupPC. [Fork on GitHub][1].
*   A **BackupPC Windows Client** used to live at michaelstowe.com/backuppc/ but that project seems dead (last checked 2020-12-23)
*   [Stale NFS Mount Causes BackupPC `fileListReceive()` Failure][2]

<!-- Links -->
[1]: https://github.com/marios-zindilis/backuppc-archive/ "BackupPC Archive"
[2]: /posts/stale-nfs-causes-backuppc-filelistreceive-failure/ "Stale NFS Mount Causes BackupPC fileListReceive() Failure"
