---
title: "Cisco: clock"
Description: Notes on the 'clock' command on Cisco devices
Author: Marios Zindilis
first-published: 2013-12-29
---

With `clock` you can set the current time and date on a Cisco device. 
For example:

    clock set 20:08:56 Sept 21 2010

The generic syntax of the command is:

    clock set hh:mm:ss MONTH <0-31> YYYY

...where:

*   `hh` is the hour,
*   `mm` are the minutes,
*   `ss` are the seconds,
*   `MONTH` is the month,
*   `<0-31>` is the date, an integer between 0 and 31, and 
*   `YYYY` is the year.

Note that the month and date are also acceptable in the reverse order, 
for example:

    clock set 20:08:56 21 Sept 2010
