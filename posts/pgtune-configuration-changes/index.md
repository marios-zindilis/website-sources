---
title: PostgreSQL Configuration Changes with pgtune
Description: Example output from pgtune
first-published: 2013-10-25
last-modified: 2013-11-23
---

This is an example run of the `pgtune` script, which examines a system's 
hardware and proposes changes to PostgreSQL configuration, to better 
utilize the capabilities of the hardware for the benefit of the database.

The following configuration differences are from an example of a VM 
running with 2x Intel Quad @2.33GHz CPUs and 2GB of RAM. The commands 
run to create the new configuration file with pgtune were:

    pgtune -i /var/lib/pgsql/9.2/data/postgresql.conf -o /var/lib/pgsql/9.2/data/postgresql2.conf
    mv postgresql.conf postgresql-factory-default.conf
    mv postgresql2.conf postgresql.conf
    chown postgres:postgres postgresql.conf
    service postgresql restart

Differences
-----------
    < max_connections = 100
    > max_connections = 80
    ---
    < shared_buffers = 32MB
    > shared_buffers = 448MB
    ---
    < #default_statistics_target = 100	
    > default_statistics_target = 50 
    ---
    < #maintenance_work_mem = 16MB
    > maintenance_work_mem = 112MB 
    ---
    < #constraint_exclusion = partition
    > constraint_exclusion = on 
    ---
    < #checkpoint_completion_target = 0.5
    > checkpoint_completion_target = 0.9 
    ---
    < #effective_cache_size = 128MB
    > effective_cache_size = 1408MB 
    ---
    < #work_mem = 1MB
    > work_mem = 11MB
    ---
    < #wal_buffers = -1
    > wal_buffers = 8MB 
    ---
    < #checkpoint_segments = 3
    > checkpoint_segments = 16 
