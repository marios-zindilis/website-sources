---
title: Set up an HTTP Repository from RHEL DVD
description: How to configure an HTTP accessible repository from the RHEL DVD
first-published: 2014-04-05
---

<abbr title='RedHat Enterprise Linux'>RHEL</abbr> can be installed from 
various different sources. One of them is over the network, from an HTTP 
accessible repository. Here's how to create such a repository:

1.  You will first need to install `httpd` from the RHEL DVD. See 
    [Install packages from RHEL DVD with yum](/posts/rhel-yum-install-from-dvd/) 
    on how to do that.

2.  After you have installed `httpd`, enable it and start it:

        chkconfig httpd on
        service httpd start

    At this point, you should be able to open `http://localhost/` from 
    the same system on which you are working.

3.  Create a directory for the repository:

        mkdir /var/www/html/rhel

4.  Copy all the files from the DVD to the repository. Assuming that 
    either the DVD or the `.iso` image is mounted at `/media/rhel`:

        cp --recursive --archive /media/rhel/. /var/www/html/rhel/

5.  Change the SELinux context of the files in the repository:

        chcon --recursive --reference=/var/www/html/ /var/www/html/rhel/

6.  At this point the repository is only accessible from the system on 
    which it runs, since `iptables` by default does not allow HTTP 
    traffic from other hosts. To open this access, edit your 
    `/etc/sysconfig/iptables` and add this line before the `COMMIT` 
    command:

        -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT

    ...and reload the firewall:

        service iptables reload

    Alternatively, you can do from the command line:

        iptables -I INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
        service iptables save
