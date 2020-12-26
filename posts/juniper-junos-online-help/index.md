---
title: Juniper JUNOS Online Help
first-published: 2013-11-05
description: Notes on the help available on the Juniper JUNOS command line
---

Juniper devices have extensive help options in the CLI.

Tip of the day
--------------

To view a very short tip, execute:

    marios@juniper> help tip cli

This is not exactly a tip *of the day* - since you actually get a new tip each time you execute that command. Still
pretty cool though. 

Help with syslog messages
-------------------------

You can view an explanation of what a syslog message means with `help syslog`. For example, for the `LOGIN_INFORMATION`
message, execute:

    marios@juniper> help syslog LOGIN_INFORMATION

Search in help topics
---------------------

You can search in help topics with `help apropos`. This will return a list of help pages that contain the word you
searched for. For example, you can search for all pages that refer to `syslog` with:

    marios@juniper> help apropos syslog

Topical help
------------

To get help on a specific topic, start with `help topic`. For example, for help on `syslog`, execute:

    marios@juniper> help topic system syslog
