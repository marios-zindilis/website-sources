---
title: syslog
Description: syslog
first-published: 2013-12-31
---

`syslog` facilities
-------------------

*   `auth`, a.k.a. `security`
*   `authpriv`
*   `cron`
*   `daemon`
*   `kern`
*   `lrp`
*   `mail`
*   `mark`
*   `news`
*   `security`
*   `syslog`
*   `user`
*   `uucp`
*   `local0` to `local7`

`syslog` priorities
-------------------

A list of `syslog` priorities, sorted by importance, ascending:

1.  `debug`
2.  `info`
3.  `notice`
4.  `warning` (previously `warn`, now deprecated)
5.  `err` (previously `error`, now deprecated)
6.  `crit`
7.  `alert`
8.  `emerg` (previously `panic`, now deprecated)

Send test message to remote syslog server
-----------------------------------------

    echo '<12>sourcehost message text' | nc -v -u -w 0 12.34.56.78 514

[Source](http://nelsonslog.wordpress.com/2013/04/19/faking-out-remote-syslog-via-netcat/)
