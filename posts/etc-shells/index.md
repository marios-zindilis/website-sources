---
title: /etc/shells
first-published: 2014-03-11
Last Updated: 2015-01-17
description: Notes on the /etc/shells file
---

The `/etc/shells` file is a list of valid shells on the system. Example from an Ubuntu 12.10 Desktop system:

    # /etc/shells: valid login shells
    /bin/sh
    /bin/dash
    /bin/bash
    /bin/rbash
    /usr/bin/screen

Example from a CentOS 6 minimal server:

    /bin/sh 
    /bin/bash
    /sbin/nologin
    /bin/dash

See Also
--------

*   [What `/etc/shells` is and isn't](http://utcc.utoronto.ca/~cks/space/blog/unix/EtcShellsUsage)
