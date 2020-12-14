---
title: Preparing a new CentOS 6 Server
Description: Steps to prepare a fresh CentOS 6 Server for production
first-published: 2013-11-25
last-modified: 2014-07-20
---

These are some reminders to myself, to apply my personal preferences when 
installing a new server based on CentOS 6.

1.  Make booting verbose, remove the portion `rgbh quiet` from the 
    `kernel` line in `/boot/grub/grub.conf`.

2.  Install misc tools:

        yum -y install vim
        yum -y install wget
        yum -y install mlocate
        yum -y install iptraf
        yum -y install iotraf
        yum -y install man
        yum -y install nmap

3.  CRON Configuration: Sometimes fresh Minimal CentOS installations don't 
    come with Cron pre-installed. In such a case: 

        yum -y install cronie cronie-anacron crontabs

    Then activate and enable the service:

        chkconfig crond on
        service crond start

4.  SNMP Configuration:

        yum -y install net-snmp
        chkconfig snmpd on

    In `/etc/snmp/snmpd.conf` change the community to something other than the 
    default `public`, for example change the line:

        com2sec notConfigUser  default       public

    To something like:

        com2sec notConfigUser  default       My_pr1vate_c0mmunity

    Also, if you are using an NMS like Zenoss that can poll all sorts of 
    information from the device via SNMP, you can greatly expand the range of 
    data available with commenting out the following two lines...

        view    systemview    included   .1.3.6.1.2.1.1
        view    systemview    included   .1.3.6.1.2.1.25.1.1

    ...and substituting them with:

        view    systemview    included   .1

5.  Disable console blanking by editing `/boot/grub/grub.conf` and adding the 
    following at the end of the `kernel` line:

        consoleblank=0

Sources
-------

*   For the console blanking tip: [You want to turn console blanking off on your Linux servers](http://utcc.utoronto.ca/~cks/space/blog/linux/ConsoleBlankingOff).
