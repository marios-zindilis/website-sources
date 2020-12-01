---
title: yum
Description: Notes on yum
first-published: 2014-03-07
---

## Group Installations ##

On CentOS, RHEL and probably other rpm-based distributions, it is 
possible to install sets of packages that are teamed together in 
logical **groups**. These groups will install all packages that are 
required for a specific function.

You can get a list of available package groups with:

    yum grouplist

You can get a description of a group and the list of the packages that 
will be installed with:

    yum groupinfo 'Group Name'

...for example:

    yum groupinfo 'SNMP Support'

Finally, you can install a group of packages with:

    yum groupinstall 'Group Name'
