---
title: Samba security Options
description: Options for the security setting of Samba
first-published: 2013-10-25
last-modified: 2013-11-23
---

The `security` option in the `[Global]` section of Samba configuration 
defines what type of authentication is required for users to access 
shares. There are five possible values for this option: Share, User, 
Server, Domain and ADS.

Of the five possible values, the last three (Server, Domain and ADS) 
delegate authentication to another server on the network, and thus 
require additional configuration.

`security = Server`
-------------------
This option makes the Samba server ask another server to authenticate 
the user before granting access to the requested share.

`security = Domain`
-------------------
This option makes the Samba server a full member of a Domain, and asks 
a Domain Controller to authenticate the user before granting access to 
the requested share. A couple more options are required for Domain 
security to work: `workgroup` (to define the domain), `password server` 
(to define the Domain Controller) and `encrypt passwords` to be set to 
`Yes` (or `True` or `1`).

It is also required that the Samba server is joined in a Domain, which 
can be done by any user with Domain Administration privileges (in the 
following example, the user obiwan):

    net join member -U obiwan

`security = ADS`
----------------
This option works similar to the `Domain` option, and requires the same 
additional options to be set, but uses an Active Directory server for 
authentication, and utilizes newer AD protocols.
