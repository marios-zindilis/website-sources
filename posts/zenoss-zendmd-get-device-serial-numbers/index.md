---
title: "Get all device Serial Numbers from ZenDMD in Zenoss"
description: "How to get all devices' serial numbers from Zenoss with ZenDMD"
first-published: 2013-11-12
last-updated: 2014-01-15
tags:
- Zenoss
---

To get a list of devices in Zenoss that have a **Serial Number** 
defined, run the following script as user `zenoss`. It will display the 
Device Title, the Serial Number, and -as an added bonus- the Hardware 
Manufacturer.

```python
import Globals
from Products.ZenUtils.ZenScriptBase import ZenScriptBase
DMD = ZenScriptBase(connect=True).dmd

Devices = dmd.Devices.getSubDevicesGen()

for Device in Devices:
    if Device.getHWSerialNumber():
        print Device.title, Device.getHWSerialNumber(), Device.getHWManufacturerName()
```
