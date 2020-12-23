---
title: Display network routes on Linux
Description: How to view network routes on Linux
first-published: 2013-11-17
---

With `netstat`
--------------

Run `netstat -r` or `netstat --route`. Add `-n` or `--numeric` for 
numeric output. Example from a laptop, on which all traffic is routed 
out the wireless interface:

    marios@rocko ~ $ netstat -rvn
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
    0.0.0.0         192.168.88.1    0.0.0.0         UG        0 0          0 wlan0
    169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 wlan0
    192.168.88.0    0.0.0.0         255.255.255.0   U         0 0          0 wlan0

With `route`
------------

Run `route` with optional `-n` for numeric output. The result is almost 
identical to that of `netstat`. Example from the same machine as above:

    marios@rocko ~ $ route -n
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         192.168.88.1    0.0.0.0         UG    0      0        0 wlan0
    169.254.0.0     0.0.0.0         255.255.0.0     U     1000   0        0 wlan0
    192.168.88.0    0.0.0.0         255.255.255.0   U     9      0        0 wlan0

With `ip`
---------

Run `ip route`. Example from the same machine as above:

    marios@rocko ~ $ ip route
    default via 192.168.88.1 dev wlan0  proto static 
    169.254.0.0/16 dev wlan0  scope link  metric 1000 
    192.168.88.0/24 dev wlan0  proto kernel  scope link  src 192.168.88.253  metric 9

