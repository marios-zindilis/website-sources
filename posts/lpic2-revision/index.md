---
title: LPIC-2 Revision
description: A list of topics to revise before the LPIC-2 exam.
first-published: 2014-08-10
---

My list of topics to revise, before taking the LPIC-2 exam. Note that this 
list was compiled before the exam objectives' revision in October 2013.

DHCP
----
* Example Servers: bigpie1, sds1, nicds1, officesrv
* CentOS Install DHCP
* dhcpd.conf

LDAP
----
* Example Servers: ldap-master, ldap-slave
* Locate and examine LDIF files
* ldapadd, slappasswd, ldappasswd, ldapdelete, getent, ldapsearch

Routing
-------
* /proc/sys/net/ipv4/ip_forward and sysctl.conf equivalent
* RIP with routed

iptables
--------
* iptables chains, default policies
* flush tables, add rules
* configure NAT

SSH
---
* /etc/ssh/sshd_config
* Generate SSH keys with ssh-keygen
* Copy files scp
* Connect with ssh-agent
* Port Tunnelling with SSH
* Securing SSH with TCP Wrappers

Samba
-----
* /etc/samba/smb.conf
* /etc/samba/username.map
* Samba Password Options
* nmblookup -MS-
* Configure Shares with Samba
* Configure Printer Shares with Samba
* testparm, nmblookup, smbstatus, smbclient, mount -t cifs

NFS
---
* Support programs: rpc.idmapd, rpc.mount.d, rpc.nfsd, rpc.statd, portmap
* Setting up exports with NFS in /etc/exports, and on the fly with exportfs
* showmount, nfsstat, rpcinfo
* Mount over NFS

FTP
---
* Passive Vs Active FTP Modes
* CentOS install Pure-FTPd
* /etc/pure-ftpd/pure-ftpd.conf
* CentOS install vsftpd
* /etc/vsftpd/vsftpd.conf

Apache
------
* /etc/httpd/conf/httpd.conf
* access.conf, ports.conf, mime.types, magic, mods-available, mods-enabled
* Enabling user homes
* Service Virtual Domains
* Enabling CGI, PHP, Perl
* Enabling SSL
* User access with htpasswd, mod_auth
* apache2ctl
* Apache Logs

Squid
-----
* Example Servers: proxy1.linux.dom (kong), proxy2.linux.dom
* Squid configuration
* Client configuration for proxy

Sendmail
--------
* Sendmail configuration files: /etc/mail/sendmail.mc -> /etc/mail/sendmail.cf, access.db, aliases.db
* Configure hostname, relay, incoming emails
* Sendmail log files

Postfix
-------
* Postfix configuration files: /etc/postfix.main.cf
* Configure incoming emails, relay
* Postfix log files

Procmail
--------
* Creating Procmail filters

Courier
-------
* Courier configuration

Dovecot
-------
* Dovecot configuration

PAM
---
* PAM Basics, PAM Stacks, Examples of PAM Modules
* PAM Configuration, NSS Configuration

TCP Wrappers
------------
* /etc/hosts.allow, /etc/hosts.deny
* Calling TCP Wrappers from /etc/inetd
* /etc/xinetd

Snort
-----
* Network Design for SNORT

Fail2Ban
--------
* Fail2Ban configuration

Bootloaders
-----------
* Identifying active bootloader on a system (based on files on disk or with bootinfoscript)
* LILO Error Messages
* GRUB Legacy Error Messages
* GRUB2 Error Messages and Rescue Console

Boot Time Troubleshooting
-------------------------
* initrd
* dmesg
* /etc/inittab, runlevel, SysV and Upstart

Hardware Troubleshooting
------------------------
* Hardware detection (/proc/cpuinfo, lspci, lsusb, lsdev, lsmod)

Log files Troubleshooting
-------------------------
* Helpful files: syslog, messages, dmesg or boot.log, secure, cron, lastlog
* Scanning Log Files with head, tail, less, grep, logcheck
* syslogd configuration
* Logrotate configuration

Software Troubleshooting
------------------------
* strace and ltrace, ldd, lsof, strings
* Library Dependencies
* Kernel options in /proc/, sysctl, /etc/sysctl.conf
* init and the process tree
* top

Troubleshooting Login and Accounts
----------------------------------
* Logins with login, XDM, KDM, GDM, getty
* Network Logins with Telnet, SSH, X, XDMCP, VNC
* Users management with usermod
* Login variables in /etc/login.defs, /etc/profile, /etc/bashrc, ~/.profile, ~/.bashrc

Cron
----
* Schedule Jobs with Cron
