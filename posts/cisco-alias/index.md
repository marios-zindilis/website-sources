---
title: "alias (Cisco command)"
description: Notes on the alias command on Cisco devices
first-published: 2013-12-29
Last Updated: 2014-01-19
---

Example: 

    cisco(config)#alias exec save copy running-config startup-config

The example above will make the word `save` an alias of the longer 
command `copy running-config startup-config`. The general form of the 
command is:

    alias MODE ALIAS COMMAND
