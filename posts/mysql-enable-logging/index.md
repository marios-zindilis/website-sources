---
title: Enable logging in MySQL
description: How to enable different types of logging in MySQL
first-published: 2013-12-07
Last Updated: 2014-09-17
---

These are some haphazardly written notes on logging in MySQL. For consistency, 
create a new subdirectory in `/var/log/` and assign ownership to the user under 
which your MySQL server is running - by default that would be the `mysql` user:

    mkdir /var/log/mysqld/
    chown mysql:mysql /var/log/mysqld

I like to name the directory `mysqld` instead of `mysql`, to clarify 
that the logs in that directory are those of the **MySQL Server** and 
not those of the MySQL Client.

Slow Query Log
--------------

The slow query log contains queries the execution of which took more 
than the value of the `long_query_time` variable, which by default is 
10 seconds. 

To enable the slow query log on the next start of `mysqld`, add the 
following line in your MySQL server's configuration file (`/etc/my.cnf` 
by default), in the `[mysqld]` section:

    slow-query-log
    slow_query_log_file=/var/log/mysqld/slow-queries.log

Create the file and assign ownership to the `mysql` user:

    touch /var/log/mysqld/slow-queries.log
    chown mysql:mysql /var/log/mysqld/slow-queries.log

Finally, either restart the server to enable the logging, or enable it 
on the already running server with:

    mysql> SET GLOBAL slow_query_log_file='/var/log/mysqld/slow-queries.log';
    mysql> SET GLOBAL slow_query_log=1;

In any of those cases, the values of the global variables 
`slow_query_log` and `slow_query_log_file`, should change from the 
default:

    mysql> show variables like 'slow_query_log%';
    +---------------------+----------------------------------+
    | Variable_name       | Value                            |
    +---------------------+----------------------------------+
    | slow_query_log      | OFF                              |
    | slow_query_log_file | /var/run/mysqld/mysqld-slow.log  |
    +---------------------+----------------------------------+

To:

    mysql> show variables like 'slow_query_log%';
    +---------------------+----------------------------------+
    | Variable_name       | Value                            |
    +---------------------+----------------------------------+
    | slow_query_log      | ON                               |
    | slow_query_log_file | /var/log/mysqld/slow-queries.log |
    +---------------------+----------------------------------+
