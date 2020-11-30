---
title: "Show privileges for all users in MySQL"
Description: Bash script for displaying privileges for all users in MySQL
first-published: 2015-07-25
tags:
- MySQL
---

Example script:

    mysql   --silent \
            --skip-column-names \
            --user mysqldumper \
            --execute 'SELECT User, Host from mysql.user' | \
            while read User Host; do 
                mysql --user mysqldumper --execute "SHOW GRANTS FOR '$User'@'$Host'"; 
                echo "==========================="; 
            done


The `mysqldumper` user only requires read permissions on the databases.
