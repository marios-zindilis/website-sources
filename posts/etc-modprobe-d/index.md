---
title: "/etc/modprobe.d/"
first-published: 2014-07-12
Last Updated: 2014-07-13
description: Notes in the /etc/modprobe.d/ director
---

On CentOS and probably other RedHat-based distributions, `/etc/modprobe.d/` 
is the directory which contains configurations for loading kernel modules, 
module aliases and module blacklists, that are specific to the host, and not 
shipped by packages (those should go in [/usr/lib/modprobe.d/](/docs/lfs/usr/lib/modprobe.d/).

Module configurations from both directories are read by default, unless 
[dracut](/docs/dracut.html) is run in *generic* mode.

See also
--------

*   [dracut](/docs/dracut.html)
*   [/usr/lib/modprobe.d/](/docs/lfs/usr/lib/modprobe.d/)
