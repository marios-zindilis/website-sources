---
title: Mount an NTFS disk in Ubuntu
first-published: 2020-12-17
---

On a system that dual boots Windows and Ubuntu, I use this line in `/etc/fstab` to have the NTFS Windows disk mounted
permanently in Ubuntu:

```
/dev/disk/by-uuid/1EBB57A121AFC448 /mnt/1TB auto nosuid,nodev,nofail,x-gvfs-show,uid=marios,gid=marios,permissions,dmask=022,fmask=133 0 0
```

Here's a list of options used in that line:

*   `nosuid`: this is a security enhancement, preventing execution of files under the files' owner's permissios, from
    users other than the owner
*   `nodev`: this prevents the mounted filesystem from containing device files
*   `nofail`: prevents error reports when the device is missing, but can mask failures
*   `x-gvfs-show`: makes the device visible in the user interface, in the file explorer
*   `uid=marios` and `gid=marios`: assign user and group ownership of new files to a specific user
*   `dmask=022` and `fmask=133`: default permissions for newly created files and directories

Image credit: *Library At Trinity College*. In the Public Domain.
[Source](https://www.publicdomainpictures.net/en/view-image.php?image=255082&picture=library-at-trinity-college).
