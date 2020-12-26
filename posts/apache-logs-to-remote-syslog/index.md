---
title: Send Apache logs to remote syslog server
first-published: 2014-02-11
description: How to send logs from Apache to a remote server
---

This document briefly describes how to send the logs from Apache to 
both a remote server, as well as log them locally. There are benefits 
to both approaches -local and remote logging- and with this method you 
will have them both.

Two types of logs are described, the `CustomLog` and the `ErrorLog`. 
Also, my preferred method of shipping logs is with `nc`, due to its 
simplicity. 

## `CustomLog` ##

The `CustomLog` is provided by the `log_config` module, which is 
included in the default installation of Apache (at least on CentOS 6.5 
and on Ubuntu 12.10 on which I tested). This module is flexible enough 
to allow multiple `CustomLog` directives, so to get Apache to log in 
both local files and to a remot syslog server you can use two lines:

    CustomLog logs/access_log combined
    CustomLog "| nc -u -j syslog.example.com 514" combined

You will need to adjust `syslog.example.com` to  your syslog server, 
and possibly `514` to whatever port your server is listening to, if 
it's not the default 514.

## `ErrorLog` ##

The `ErrorLog` is provided by the `core` module, and unfortunately it's 
not as flexible as `CustomLog`, so it doesn't allow multiple `ErrorLog` 
directives. If you add more than one, only the one that occures last in 
the configuration file will be used. Luckily, you can utilize the power 
of `tee` to overcome this:

    ErrorLog "| tee -a /var/log/httpd/error_log | nc -u -j syslog.example.com 514"

## See also ##

*   [Sending web logs to Computer Security](http://www.fnal.gov/docs/products/apache/syslog_logs_notes.html) 
    from Fermilab, source of the `tee` tip.
*   [Apache HTTPD CustomLog to Syslog via UDP](https://web.archive.org/web/20130222080252/http://mykospark.net/2011/01/apache-httpd-customlog-to-syslog-via-udp/), 
    source of the `nc -u -j` tip.
