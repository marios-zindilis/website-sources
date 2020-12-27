---
title: /etc/hosts.allow
Author: Marios Zindilis
first-published: 2013-10-25
Last Updated: 2014-01-15
description: Notes on the /etc/hosts.allow configuration file
---

<p class='lead'>File <code>/etc/hosts.allow</code> is one of the configuration 
files of <b>TCP Wrappers</b>, effective either to servers launched via a 
superserver such as <code>inetd</code>, or to servers that use 
<code>libwrap</code>, such as the OpenSSH server (when compiled with 
<code>--with-libwrap</code>, which is usually the default).</p>

The general syntax of the file is:

    server: hosts

The `hosts` is a comma separated list, that can include full IP 
addresses, partial IP addresses, IP subnets in CIDR notation, 
FQDNs, or domains. For example, the following line limits access to the 
`sshd` server to a single host specified by IP, and to a subnet:

    sshd: 8.8.8.1 10.1.0.0/24

Note that in order for any definitions in `/etc/hosts.allow` to be 
effective, the rest of the hosts **need to be prohibited** in 
`/etc/hosts.deny`. Since `/etc/hosts.allow` takes precedence, it is safe 
to disallow every host in `/etc/hosts.deny` and only selectively allow 
specific access to the server in `/etc/hosts.allow`. To deny all hosts 
except those specified in `/etc/hosts.allow`, add in `/etc/hosts.deny`:

    sshd: ALL
