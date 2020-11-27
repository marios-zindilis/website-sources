---
title: "hostname (Cisco Command)"
Description: Notes on the 'hostname' command on Cisco devices
Author: Marios Zindilis
first-published: 2014-01-05
---

The `hostname` command defines the name of the device. This is the name 
displayed (among other places) in the command prompt, and in the CDP 
neighbors list of other devices.

Example:

    cisco> enable
    cisco# configure terminal
    cisco(config)# hostname router-5
    router-5(config)# 

Juniper Equivalent
------------------

The Juniper equivalent of the `hostname` command is:

    marios@juniper# set system hostname <HOSTNAME>

See also
--------

*   [`set system host-name`](/docs/juniper/set-system-host-name.html), the Juniper equivalent
