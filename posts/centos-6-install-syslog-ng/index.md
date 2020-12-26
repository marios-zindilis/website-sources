---
title: Installation of syslog-ng on CentOS 6
description: How to install syslog-ng on CentOS 6
first-published: 2014-03-11
---

1.  Enable EPEL
2.  yum install -y syslog-ng
3.  Configure /etc/syslog-ng/syslog-ng.conf
4.  `chkconfig syslog-ng on`
5.  `chkconfig rsyslog off`
6.  `service rsyslog stop`
7.  `service syslog-ng start`
