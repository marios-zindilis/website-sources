---
title: /etc/shadow
description: Notes and Links on /etc/shadow
first-published: 2014-04-08
---

The fields, delimited by `:` are:

1.  Username
2.  Password (hashed)
3.  Last password change (in days since 1970-01-01)

Fields
------

Notes on specific fields.

### Username ###

Fairly straightforward. The lenght used to be restricted to 8 characters 
maximum in very old distributions, now it's 32 characters on most 
distributions.

### Password ###

An empty field, will allow the user to login without providing a 
password, whereas a field that starts with, or only contains an asterisk 
`*` indicates a disabled account.

The first characters of this field define the hashing algorithm, one of:

1.  MD5 - password starts with `$1$`
2.  Blowfish - password starts with `$2a$`
3.  SHA-256 - password starts with `$5$`
4.  SHA-512 - password starts with `$6$`
