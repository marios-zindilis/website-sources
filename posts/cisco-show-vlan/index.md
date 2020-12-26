---
title: "show vlan (Cisco)"
description: Notes on the `show vlan` command on Cisco devices
first-published: 2014-02-22
Last Updated: 2014-02-24
tags:
- Cisco
---

`show vlan` lists the Vlans and the ports on which each is configured. 
Trunk ports don't show up in this list. 

Vlans 1002, 1003, 1004 and 1005
-------------------------------

In the output of the `show vlan` command, Vlans 1002, 1003, 1004 and 
1005 are listed. Those exist by default to ensure interoperation and 
backwards compatibility of new devices with old and largely deprecated 
networks. For example, Vlans 1002 and 1004 were used on an old fiber 
optics type of network, whereas Vlan 1003 was used in token-ring 
networks. In the `status` column, those networks show up as either 
`act/unsup` or `active/unsupported`, since most new switches don't even 
have ports to use on those old types of networks any more.
