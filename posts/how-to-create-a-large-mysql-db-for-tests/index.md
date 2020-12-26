---
title: How to create a large MySQL database for tests
first-published: 2012-11-13
description: A simple method for creating a large test database in MySQL
---

I wanted to create a fairly big MySQL database (~30GB on disk) for 
tests. I created a '`test`' database, and a '`table2`' table, with a 
'`content`' field with datatype `TEXT`. Here's what I run from Bash:

```bash
marios@mysql-server:~$ time for i in `seq 1 500000`; \
do echo $i of 500000; \
mysql -u root -D test -e \
"INSERT INTO table2 (content) VALUES ('`cat /var/log/messages | tr -d \'`')"; \
done
```

This took a few hours to finish, while creating data at a rate of 100MBytes per minute on my test server. The size of the database was 30GB.

This did the job but it was too slow. The **better way to do it**, 
would be to run the above command for far less repetitions, say 
100000, to create a smaller table, then copy that table in the same 
database as many times as required for the entire database to reach 
the desirable size, like:

```sql
create table2 like table;
insert into table2 select * from table;
```
