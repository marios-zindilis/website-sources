---
title: enable password
description: Notes on Cisco command 'enable password'
first-published: 2014-01-02
---

The `enable password` command will make the device ask for a password 
to allow the user to move up to the global configuration mode. It has 
the disadvantage of displaying the password in clear text in the 
configuration. For this reason, is has been deprecated, in favour of 
`enable secret`, but still exists in Cisco IOS for compatibility with 
older hardware that cannot encrypt the enable password.

See also
--------

*   <a href='/posts/cisco-enable-secret/'><code>enable secret</code></a>
