---
title: Cannot set device IP in Zenoss
Description: How to fix failure to set IP in Zenoss
first-published: 2014-01-15
Last Updated: 2016-11-06
tags:
- Zenoss
---

In Zenoss, there is a chance that you might get a failure to set or change the IP of a device in the web interface.
This sometimes happens because that specific IP address is already assigned to one of the *subinterfaces* (typically a
Vlan interface) of another device. In this case, you will just get an error that Zenoss `Failed to set IP Address`. 

If you try to set or change the IP address from ZenDMD, then the error is a little bit more informative:

```python
>>> Device = find('web-server.zindilis.net')
>>> Device.setManageIp('1.2.3.4')
2014-01-15 11:18:30 WARNING zen.Device The IP address 1.2.3.4 is already assigned
'The IP address 1.2.3.4 is already assigned'
```

The problem here is that you can't find an IP by searching for it in the web interface, if that IP is assigned to a
subinterface. Enter ZenDMD! Here's how to search of a specific IP, in all the interfaces of all the devices:

```python
>>> for Device in dmd.Devices.getSubDevices_recursive():
...      for Interface in Device.os.interfaces(): 
...           if Interface.getIpAddress() != None and Interface.getIpAddress().startswith('1.2.3.4'):
...                print Device.id, Interface.id, Interface.getIpAddress()
```

Running the above, will return a result similar to:

    router-1.zindilis.net Fa0/1.900 1.2.3.4

This tells you the device on which that IP address exists, and the specific subinterface on which it is assigned to.
