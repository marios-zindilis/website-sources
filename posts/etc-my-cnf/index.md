---
title: /etc/my.cnf
language: el
description: Σημειώσεις για το αρχείο /etc/my.cnf
Author: Marios Zindilis
first-published: 2011-12-19
---

Το αρχείο `/etc/my.cnf` περιέχει τις ρυθμίσεις του MySQL Server.

Παράδειγμα αρχείου με τις προεπιλεγμένες ρυθμίσεις, από σύστημα με 
Centos 4 και MySQL Server 4:

    [mysqld]
    datadir=/var/lib/mysql
    socket=/var/lib/mysql/mysql.sock
    user=mysql
    # Default to using old password format for compatibility with mysql 3.x
    # clients (those using the mysqlclient10 compatibility package).
    old_passwords=1
     
    # Disabling symbolic-links is recommended to prevent assorted security risks;
    # to do so, uncomment this line:
    # symbolic-links=0
     
    [mysqld_safe]
    err-log=/var/log/mysqld.log
    pid-file=/var/run/mysqld/mysqld.pid
