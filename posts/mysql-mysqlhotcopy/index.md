---
title: mysqlhotcopy
description: Notes on mysqlhotcopy
first-published: 2015-01-29
tags:
- MySQL
---

**mysqlhotcopy** is a Perl script for backing up MySQL tables stored in either 
the MyISAM or ARCHIVE engines. It works very fast because it doesn't dump the 
contents of the tables. Instead it takes advantage of the fact that MyISAM 
tables are contained in separate files, and simply locks the tables and copies 
the flat files.

Example Setup
-------------

1.  Create a user in MySQL for running `mysqlhotcopy`. The user will need to be 
    granted the `SELECT`, `RELOAD` and `LOCK TABLES` privileges on the
    databases that will be backed up. In this example, I want all databases to 
    be backed up:

        mysql> CREATE USER `mysqlhotcopy`;
        mysql> GRANT SELECT, RELOAD, LOCK TABLES ON *.* TO `mysqlhotcopy`@`localhost`;

2.  Create a daily cron job to backup every database, except for 
    `information_schema`, which is a dynamic schema created my the MySQL server 
    itself and does not exist as files on the filesystem. An example script is:

        #!/bin/bash

        for database in $(mysql --user mysqlhotcopy           \
                                --batch                       \
                                --skip-column-names           \
                                --execute 'SHOW DATABASES;' | \
                          grep -v '^information_schema$')
        do
            mysqlhotcopy $database /root/mysql-dumps/ --allowold --keepold --user=mysqlhotcopy
        done

    This script maintains one previous copy of the databases, by renaming the 
    directory, appending the suffix `_old` to the name. 

See also
--------

*   [mysqlhotcopy - A Database Backup Program](http://dev.mysql.com/doc/refman/5.6/en/mysqlhotcopy.html)
