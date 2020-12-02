---
title: Get Devices from Zenoss with ZenDMD
description: How to get devices from Zenoss with ZenDMD
first-published: 2013-10-25
Last Updated: 2014-08-29
---

A simple example:

```python
#!/usr/bin/env python

import Globals
from Products.ZenUtils.ZenScriptBase import ZenScriptBase
DMD = ZenScriptBase(connect=True).dmd

Devices = DMD.Devices.getSubDevicesGen()
```

Filter by Device Class
-------------------------

If you only want to select the devices from a specific class, for 
example the class `/Servers/Linux`, you could do:

```python
for Device in DMD.Devices.getOrganizer('/Servers/Linux').getSubDevices():
    print Device.title, Device.manageIp
```

Otherwise, if you want to separate that specific class name out of a 
list of *all* the devices, you could do:

```python
for Device in DMD.Devices.getSubDevices():
    if Device.getDeviceClassName().startswith('/Server/Linux'):
        print Device.title, Device.manageIp
```

Get Devices by Group
--------------------

If you only want to select the devices from a specific group, for example the 
group `/Customers/VIP/Acme` you could do:

```python
for Device in DMD.Groups.Customers.VIP.Acme.getSubDevices():
    print Device.title, Device.manageIp
```

If your group contains whitespace, for exampe `/Customers/VIP/Acme Anvils`, 
then you can use a different approach:

```python
for Device in getattr(DMD.Groups.Customers.VIP, 'Acme Anvils').getSubDevices():
    print Device.title, Device.manageIp
```

Find Specific Device
--------------------

This example is from the `zendmd` Python interpreter, run as user `zenoss`:

    Device=find('my-big-server')
    >>> print Device.title
    my-big-server
