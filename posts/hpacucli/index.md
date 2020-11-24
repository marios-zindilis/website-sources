---
title: hpacucli
Description: HP RAID Array Configuration Linux Utilitys
first-published: 2013-12-02
last-modified: 2014-07-23
---

`hpacucli` allows control of the RAID array controllers on HP servers.

Installation on CentOS
----------------------

1.  Download RPM from HP's website (free account required).
2.  Install, e.g:

        yum install hpacucli-9.40-12.0.x86_64.rpm

Usage
-----

### Status of All Controllers ###

To display the status of all RAID controllers:

    hpacucli ctrl all show status

Example output:

    Smart Array P410i in Slot 0 (Embedded)
       Controller Status: OK
       Cache Status: OK
       Battery/Capacitor Status: OK

### Configuration of All Controllers ###

To display the configuration of all RAID controllers:

    hpacucli ctrl all show config

Example output:

    Smart Array P410i in Slot 0 (Embedded)    (sn: 5001438001AD7D90)
     
       array A (SAS, Unused Space: 0  MB)
     
     
          logicaldrive 1 (136.7 GB, RAID 1, OK)
     
          physicaldrive 1I:1:1 (port 1I:box 1:bay 1, SAS, 146 GB, OK)
          physicaldrive 1I:1:2 (port 1I:box 1:bay 2, SAS, 146 GB, OK)
     
       SEP (Vendor ID PMCSIERA, Model  SRC 8x6G) 250 (WWID: 5123432101AB8D8F)

### Status of Logical Drives ###

To display the status of all logical drives on a specific controller:

    hpacucli ctrl slot=0 ld all show status

Example output:

       logicaldrive 1 (136.7 GB, 1): OK

### Status of all physical drives ###

To display the status of all physical drives on a specific controller:

    hpacucli ctrl slot=0 pd all show status

Example output:

       physicaldrive 1I:1:1 (port 1I:box 1:bay 1, 146 GB): OK
       physicaldrive 1I:1:2 (port 1I:box 1:bay 2, 146 GB): OK

See Also
--------

*   [17 hpacucli Command Examples for Linux on HP Servers](http://www.thegeekstuff.com/2014/07/hpacucli-examples/)
