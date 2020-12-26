---
title: Skip one record on MySQL replication slave
description: A quick and dirty way to resume broken MySQL replication
first-published: 2013-11-26
Last Updated: 2013-11-26
---

MySQL replication can break for a wide multitude of reasons. The 
following is a quick and dirty way to resume replication, if the error 
that appears in the output of `SHOW SLAVE STATUS\G` indicates that 
replication stopped at a specific record. 

It is a <i>dirty</i> way, because immediately after using it the 
database on the slave becomes different from that on the master by one 
record. If you can live with that, the skip-one method can help you 
quickly put the slave back in track.

On the MySQL CLI:

    mysql> STOP SLAVE;
    mysql> SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1;
    mysql> START SLAVE;
    mysql> SHOW SLAVE STATUS\G

Hopefully your replication will resume after this, and the value of 
your `Seconds_Behind_Master` will quickly be depleted.
