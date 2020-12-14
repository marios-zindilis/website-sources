---
title: Create an encrypted directory on Ubuntu
Description: How to create an encrypted directory with encfs on Ubuntu
first-published: 2014-02-04
last-modified: 2015-07-25
---

These are very brief instructions on how to create an encrypted directory on 
an Ubuntu system. A usage scenario for this, is that you can keep your files 
encrypted while stored on a cloud storage service, and decrypt them on-demand 
only when necessary.

Install encfs:

    sudo apt-get install encfs

Link encrypted and decrypted direcories:

    encfs ~/.encrypted ~/visible

If the directories don't exist, encfs will ask to create them.

To umount:

    fusermount -u ~/visible

To re-mount:

    encfs ~/.encrypted ~/visible
