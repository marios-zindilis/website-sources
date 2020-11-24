---
title: Install Oracle Virtualbox on Linux Mint 17
description: How to install the closed source version of Oracle Virtualbox on Linux Mint 17
first-published: 2014-10-01
last-modified: 2015-03-23
---

This is a quick tip on how to install the closed-source version of Virtualbox, currently at version 4.3.16, on a
machine with Linux Mint 17.

<!-- read more -->

The open-source version of Virtualbox is available in the default repositories for Linux Mint, but the version offered
at the Virtualbox website itself is newer, and in fact easier to install.

So, here we go:

1.  Remove previously installed Virtualbox packages:

        sudo apt-get purge virtualbox*

2.  Install dependencies:

        sudo apt-get install dkms
        sudo apt-get install linux-headers-`uname -r`

3.  Download Virtualbox from [Virtualbox Downloads](https://www.virtualbox.org/wiki/Linux_Downloads).

4.  Install with `dpkg` (note that the filename might be different): 

        sudo dpkg -i Downloads/virtualbox-4.3.deb

See also
--------

*   Check [virtualbox.org](https://www.virtualbox.org/) for the latest version.

<hr>

Image credit: Edited from *Ferns of Great Britain and Ireland: Polypodium Phegopteris*, by Henry Bradbury. In the
Public Domain. [Source](https://archive.org/details/clevelandart-1993.45-ferns-of-great-brita).
