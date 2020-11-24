---
title: Change pptpd port on CentOS
description: How to change pptpd listening port on CentOS
first-published: 2014-09-17
last-modified: 2014-09-20
---

I was testing Point-to-Point Tunneling today, between a CentOS 6 server running `pptpd`, and some Mikrotik routers as
the clients, and I wanted to change the listening port on the `pptpd` side. After searching and reading a bit of 
documentation for a while, I was surprised to find that `pptpd` respects the record for `pptp` as defined in 
`/etc/services`.

<!-- read more -->

Therefore, to change the port on which `pptpd` listens, change the following 
two lines:

    pptp            1723/tcp                # pptp
    pptp            1723/udp                # pptp

...from the default 1723 to whatever you want, and restart the `pptpd` service. 

Just in case, if you want to use a port that is already defined differently in 
`/etc/services`, you might comment out the default entries to avoid confusion. 
In my case, I used port 1821, so I commented out all the following lines:

    # pptp            1723/tcp                # pptp
    # pptp            1723/udp                # pptp
    # donnyworld      1821/tcp                # donnyworld
    # donnyworld      1821/udp                # donnyworld

...and added:

    pptp            1821/tcp                # pptp
    pptp            1821/udp                # pptp

A bit more on `/etc/services`
-----------------------------

Quoting the `man` page for `services`:

> **services**  is  a  plain ASCII file providing a mapping between 
> human-friendly textual names for internet services, and their underlying 
> assigned port numbers and protocol types.  Every networking program should 
> look into this file to get the port number (and protocol) for its service.

So, `pptpd` is actually doing the right thing here, looking at this file to 
find its port. I wonder which other servers do the same thing.

<hr>

Image credit: Part of *Variable-Density Tunnel - Wind Tunnel #2*, by NASA. In the Public Domain.
[Source](https://archive.org/details/NIX-EL-1999-00272)
