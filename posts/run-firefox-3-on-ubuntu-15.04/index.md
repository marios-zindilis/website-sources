---
title: How to run Firefox 3.6 on Ubuntu 15.04
description: Instructions on how to run the old 3.6 version of Firefox on a new version of Ubuntu
first-published: 2015-09-05
tags:
- Firefox
- Ubuntu
---

These instructions will allow you to run the ancient 3.6 version of Firefox on a recent Ubuntu installation, namely 
15.04, but it could apply to versions of Debian, Ubuntu and Linux Mint released close to 15.04.

<!-- read more -->

The reasons why *you* might want to run such an old version of Firefox are 
irrelevant to this post. For me, this solves a problem of very limited scope: 
having to run some browser tests, written in Javascript as bookmarklets, that 
only last executed correctly in Firefox 3.6. Those tests access user 
information that is not available to the Javascript engine in versions of 
Firefox newer that 3.6, since Mozilla tightened its security and it is not 
exposing the user's visited history any more.

Now, I suppose I could migrate my tests out of the browser, read the browsing 
history from some SQLite file in the user's Firefox profile, and simulate the 
browser with something like Selenium, but I just cannot be bothered.

The guide
---------

1.  Download `firefox-3.6.tar.bz2` from [ftp.mozilla.org][mozilla-ftp]. 
    Decompressing this archive will give you a directory named `firefox`.

2.  Move the `firefox` directory in `/opt/`. The target of these instructions 
    is to get `/opt/firefox/firefox` to execute without errors.

3.  Trying to run `/opt/firefox/firefox` now, results in 'library missing' 
    errors for `libgtk-2.0-0` and `libdbus-glib-1-2`. Both these libraries 
    exist in an Ubuntu 15.04 installation, but they are 64bit libraries 
    whereas Firefox 3.6 was only ever released as a 32bit application.

    Both problems are solved by installing the 32bit versions of those 
    libraries:
    
        sudo apt-get install libgtk-2.0-0:i386
        sudo apt-get install libdbus-glib-1-2:i386

4.  Run `/opt/firefox/firefox` now and you should be able to enjoy the retro 
    experience of times gone by, with no Flash or any other plugin for that 
    matter. A note of caution: running such an old version of a browser is 
    very unsafe. Don't do anything other than testing with it, use a clean 
    profile (run with `-P` option and create a test profile), and if possible, 
    sandbox the application so that it can't touch anything on your main 
    system.

A note about library paths: Firefox 3.6 looks for libraries into its 
installation directory (in this case `/opt/firefox/firefox`), in addition to 
directories in the library path. Therefore, if you hit an issue where the 
browser can't locate libraries that exist on the system, it is easier and 
probably safer to create symbolic links to those libraries in 
`/opt/firefox/firefox` rather than altering your library path just to 
accommodate the needs of this old application. 

Enjoy!

<!-- Links -->
[mozilla-ftp]: https://ftp.mozilla.org/pub/firefox/releases/3.6/linux-i686/en-US/


