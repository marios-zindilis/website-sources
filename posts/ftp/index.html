---
title: FTP and ftp
Description: Notes on the FTP protocol and the ftp utility
first-published: 2014-01-07
---

These are some notes on the <abbr title='File Transfer Protocol'>FTP</abbr> 
protocol, and on the `ftp` command line client.

Active Vs Passive Mode
----------------------

Two network connection are required for FTP transfers. In **active 
mode**, the client initiates a connection to the server's port 21, and 
the server initiates a connection back to the client's port 21. This 
incoming connection (from the client's point of view) is often blocked 
by firewalls, and/or not available in <abbr title="Local Area Network">
LANs</abbr> that are <abbr title="Network Address Translation">NAT</abbr>ed. 
To overcome this problem, in **passive mode**, the client initiates 
both connections to the server.

In the `ftp` CLI utility, the default mode is active. You can switch 
between modes with the `passive` command:

    marios@zindilis.net ~ $ ftp
    ftp> open ftp.zindilis.net
    Name (ftp.zindilis.net:marios): 
    331 Please specify the password.
    Password: 
    ftp> passive
    Passive mode on.
    ftp> passive
    Passive mode off.
    ftp> 

There is no `active` command, modes are toggled with `passive`.

ASCII Vs Binary Transfers
-------------------------

There are two transfer modes, ASCII and Binary. In **ASCII**, files are 
transferred as characters, some of which (like newlines) are converted 
to match the operating system of the client. This works well for text 
files, but is almost certain to corrupt any other type of file. In 
**binary** mode, files are transferred unaltered. This helps maintain 
the integrity of files, but increases the possibility of text files 
appearing broken when transferred between different operating systems. 

In the `ftp` CLI utility, the commands `ascii` and `binary` switch 
transfer modes:

    marios@zindilis.net ~ $ ftp
    ftp> open ftp.zindilis.net
    Name (ftp.zindilis.net:marios): 
    331 Please specify the password.
    Password:
    ftp> ascii
    200 Switching to ASCII mode.
    ftp> binary
    200 Switching to Binary mode.
    ftp> 

