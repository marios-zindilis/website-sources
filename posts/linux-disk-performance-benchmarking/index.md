---
title: Disk Performance Benchmarking in Linux
Description: How to do disk performance benchmarks in Linux
first-published: 2013-11-14
last-modified: 2014-02-04
---

This page is a dump of notes on **disk benchmarking in Linux**, and is in great need of editing! Use at own peril.

*  http://www.westnet.com/~gsmith/content/postgresql/pg-disktesting.htm
*  http://archive09.linux.com/feature/139742

## `hdparm` ##

### Buffered disk reads (bypasses cache) ###

This method reads data sequentially from the disk, disregarding file boundaries.

```bash
mariosz@super-mario:~$ sudo hdparm -t /dev/sdb
/dev/sdb:
 Timing buffered disk reads: 228 MB in  3.02 seconds =  75.52 MB/sec
```

### Cached reads ###

```bash
mariosz@super-mario:~$ sudo hdparm -T /dev/sdb
/dev/sdb:
 Timing cached reads:   3158 MB in  2.00 seconds = 1579.16 MB/sec
```

## `dd` ##

### Writes ###

```bash
mariosz@super-mario:~$ dd if=/dev/zero of=testfile bs=8k count=100k; rm -f testfile
102400+0 records in
102400+0 records out
838860800 bytes (839 MB) copied, 12.7363 s, 65.9 MB/s
```

### Writes and Cache-out ###

```bash
mariosz@super-mario:~$ time sh -c "dd if=/dev/zero of=testfile bs=8k count=100k; sync" ; rm -f testfile
102400+0 records in
102400+0 records out
838860800 bytes (839 MB) copied, 12.6832 s, 66.1 MB/s
real	0m19.304s
user	0m0.020s
sys	0m1.232s
```

## Install iozone ##

1.  Go to [http://www.iozone.org/], get link of latest build.
2.  wget http://www.iozone.org/src/current/iozone-3-420.i386.rpm
3.  yum install iozone-3-420.i386.rpm

If you also need to create graphs from the results of `iozone`, you can additionally install `gnuplot`:

```
yum install gnuplot
```

Using iozone
------------
*   [http://www.thegeekstuff.com/2011/05/iozone-examples/]

Bonnie++
--------

The `bonnie++` benchmark is available on [EPEL](/posts/centos-6-enable-epel-repository/) repository for CentOS. Note
that it will put load on the system on which it runs, so it's better run during less productive times.

```bash
[root@centos-6-3-minimal ~]# bonnie++ -u root
```

Bonnie++ will output its data twice, once in an ASCII formatted table, and once more in a single line CSV. Echo the
contents of the CSV line through bon_csv2html to get a more readable HTML table.
