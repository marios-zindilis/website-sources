---
title: /etc/sysconfig/network-scripts/ifcfg-eth0
description: The ifcfg-eth0 network configuration file
first-published: 2013-11-26
Last Updated: 2013-11-26
---

Example of a network interface configuration file:

    # Hardware configuration:
    UUID="46e26279-f69b-daf5-1b79-d03677b7178d"
    HWADDR=00:1C:C0:0D:36:C6
    DEVICE="p1p1"
    TYPE="Ethernet"
    ONBOOT="yes"
    NAME=eth0
    NM_CONTROLLED="no"
     
    # IPv4 configuration:
    BOOTPROTO="none"
    IPADDR0="192.168.1.100"
    PREFIX0="24"
    GATEWAY0="192.168.1.1"
    DNS1="192.168.1.53"
    DEFROUTE="yes"
    IPV4_FAILURE_FATAL="yes"
     
    # IPv6 configuration:
    IPV6INIT="yes"
    IPV6_AUTOCONF=yes
    IPV6_DEFROUTE=yes
    IPV6_FAILURE_FATAL=no
    IPV6_PEERDNS=yes
    IPV6_PEERROUTES=yes

Parameters
----------

<dl class='dl-horizontal'>
<dt><code>BOOTPROTO</code></dt>
<dd>Can be set to <code>dhcp</code> if the interface will be 
automatically configured by a DHCP server, or to <code>static</code> if 
the interface will have a static IP configuration, or to 
<code>none</code>.</dd>
<dt><code>DEFROUTE</code></dt>
<dd>If set to <code>yes</code>, then during the activation of the 
interface, a default route towards the associated gateway will be added 
on the system.</dd>
<dt><code>HWADDR</code></dt>
<dd>The MAC Address of the interface, (<code>HWaddr</code> in the 
output of <code>ifconfig</code>), physically burnt on the device by the 
manufacturer.</dd>
<dt><code>NM_CONTROLLED</code></dt>
<dd>Whether or not the interface is controlled by the NetworkManager 
service. If set to <code>no</code>, the operation of the interface needs 
to be controlled elsewise, e.g. with running the <code>network</code> 
service in the place of NetworkManager.</dd>
<dt><code>ONBOOT</code></dt>
<dd>Whether or not the interface should be activated automatically 
during system startup.</dd>
</dl>
