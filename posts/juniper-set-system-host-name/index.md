---
title: "set system host-name (Juniper Command)"
description: Notes on the 'set system host-name' command on Juniper devices
Author: Marios Zindilis
first-published: 2014-01-05
---

The `set system host-name` command defines the name of the device, as 
displayed (among other places) in the command prompt. For example:

    marios@juniper> configure
    marios@juniper# set system host-name router-4
    marios@juniper# commit
    commit complete
    marios@router-4#

Cisco Equivalent
----------------

The Cisco equivalent of the `set system host-name` command is: 

    cisco# hostname router-5

See also
--------

*    [`hostname`](/posts/cisco-hostname/), the Cisco equivalent
