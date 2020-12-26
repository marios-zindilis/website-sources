---
title: "Zenoss: Add a device to a Group with ZenDMD"
first-published: 2014-02-19
Last Updated: 2016-11-06
description: How to add a device to a group in Zenoss, using the ZenDMD shell.
tags:
- Zenoss
---

As user `zenoss` (or whatever user your Zenoss application runs as), run `zendmd`, and:

```python
Device = find('My Awesome Server')
Device.addDeviceGroup('My Wonderful Group')
commit()
```

This will make the device named `My Awesome Server` a member of the group named `My Wonderful Group`. 
