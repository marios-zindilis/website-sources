---
title: tailf
Description: Notes on the 'tailf' command
first-published: 2014-01-02
---

`tailf` does the same thing as `tail -f`: it displays the last lines of 
a file, and then *follows* the file as it grows, and displays new lines 
as they are appended to it. Compared to `tail -f`, it uses less 
resources on the system, by not reading from the disk while the text 
file is not updated.

See also
--------

*   [`tail`](/docs/tail.html)
