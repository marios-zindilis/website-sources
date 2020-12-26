---
title: dhcrelay
description: DHCP relay dhcrelay
first-published: 2013-12-31
---

`dhcrelay` acts as a proxy between DHCP clients and a DHCP server, when 
those are separated by a router, which by default blocks broadcasted 
DHCP requests. It makes sense to install this on a router between two 
subnets, only one of which has a DHCP server, or on a relay system, 
again between two subnets.
