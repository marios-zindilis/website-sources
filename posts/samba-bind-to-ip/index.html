---
title: Bind Samba server to specific IP
Description: How to bind the Samba server to a specific interface or IP
first-published: 2014-03-14
---

By default, a Samba server will listen for connections on **all** the 
interfaces of the host. This can be verified by:

    # netstat -tulpn | egrep "samba|smbd|nmbd|winbind"
    tcp    0    0 0.0.0.0:139         0.0.0.0:*       LISTEN      30352/smbd
    tcp    0    0 0.0.0.0:445         0.0.0.0:*       LISTEN      30352/smbd
    tcp    0    0 :::139              :::*            LISTEN      30352/smbd
    tcp    0    0 :::445              :::*            LISTEN      30352/smbd

To bind the server to a specific IP or interface, add the following 
lines to the `[global]` section of Samba's configuration:

    bind interfaces only = yes
    interfaces = lo eth0

That will make the server listen to connections only to `lo` and `eth0`. 
The value of the `interfaces` parameter, can also be an IP, like:

    interfaces = 10.1.2.3

For further information, see the `man` page for `smb.conf`.

See also
--------

*   The [Samba port usage](http://wiki.samba.org/index.php/Samba_port_usage)
    article in the Samba Wiki explains a bit more on why it might be 
    necessary to have the server listening on the `lo` interface.
