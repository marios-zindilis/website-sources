---
title: route
first-published: 2014-03-07
description: Notes on the route tool
---

Add route to specific network:

    route add -net 1.2.3.0 netmask 255.255.255.0 dev eth0

Add default gateway:

    route add default gw 1.2.3.4 eth0
