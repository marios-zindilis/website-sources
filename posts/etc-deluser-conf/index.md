---
title: /etc/deluser.conf
description: /etc/deluser.conf defaults for deluser
first-published: 2012-02-16
Last Updated: 2014-01-15
---

The `/etc/deluser.conf` configuration file contains the defaults for 
`deluser`, the tool for deleting users from a system usually found on 
Debian-based systems. An example of the file:

    # /etc/deluser.conf: `deluser' configuration.
     
    # Remove home directory and mail spool when user is removed
    REMOVE_HOME = 0
     
    # Remove all files on the system owned by the user to be removed
    REMOVE_ALL_FILES = 0
     
    # Backup files before removing them. This options has only an effect if
    # REMOVE_HOME or REMOVE_ALL_FILES is set.
    BACKUP = 0
     
    # target directory for the backup file
    BACKUP_TO = "."
     
    # delete a group even there are still users in this group
    ONLY_IF_EMPTY = 0
     
    # exclude these filesystem types when searching for files of a user to backup
    EXCLUDE_FSTYPES = "(proc|sysfs|usbfs|devpts|tmpfs)"
