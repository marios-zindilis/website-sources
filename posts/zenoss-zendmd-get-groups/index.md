---
title: Get Device Groups in Zenoss with ZenDMD
description: How to get device groups from Zenoss with ZenDMD
first-published: 2013-11-11
Last Updated: 2014-01-26
tags:
- Zenoss
---

```python
import Globals
from Products.ZenUtils.ZenScriptBase import ZenScriptBase
DMD = ZenScriptBase(connect=True).dmd
   
for Group in DMD.Groups.getSubOrganizers():
    print Group.getPrimaryId()
```

Get all Devices in specific Group
---------------------------------

```python
for Device in Group.devices():
    print Device.getPrimaryId()
```
