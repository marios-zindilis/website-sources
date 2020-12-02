---
title: Install Motion on CentOS 6
Description: How to install Motion camera monitor on CentOS 6
first-published: 2013-11-25
last-modified: 2013-11-26
---

These are some very very brief instructions on how to install 
[Motion](http://www.lavrsen.dk/foswiki/bin/view/Motion/WebHome) 
on a CentOS 6 system. 

Install Prerequisites
---------------------

    yum -y install gcc make
    yum -y install libjpeg-turbo libjpeg-turbo-devel

Install Motion
--------------

Download Motion, e.g. as a `.tgz` from SourceForge:

    cd /root/
    wget http://downloads.sourceforge.net/project/motion/motion%20-%203.2/3.2.12/motion-3.2.12.tar.gz?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fmotion%2Ffiles%2Fmotion%2520-%25203.2%2F3.2.12%2F&ts=1385393313&use_mirror=garr

Untar:

    tar zxvf motion-3.2.12.tar.gz

Install:

    cd /root/motion-3.2.12/
    ./configure
    make
    make install

This will install motion in its default location, in subdirectories of 
`/usr/local/`, e.g:

*   `/usr/local/bin/` for executables,
*   `/usr/local/man/` for man pages,
*   `/usr/local/etc/` for configuration files,
*   `/usr/local/docs/` for documentation,
*   `/usr/local/share/` for configuration examples, etc.

Finally, before starting `motion` you need to copy the default 
configuration file `motion-dist.conf` to `motion.conf` (by default in 
`/usr/local/etc/`.
