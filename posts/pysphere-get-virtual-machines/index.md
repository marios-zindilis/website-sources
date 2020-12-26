---
title: Get Virtual Machines from VMware with PySphere
description: How to get a list of Virtual Machines from a VMware host with PySphere
first-published: 2013-12-27
---

This is a simple example of how to get a list of Virtual Machines from 
a VMware host, using [PySphere](/posts/pysphere/).

{% highlight python %}
import pysphere

server = pysphere.VIServer()
server.connect('vmware.zindilis.net', 'username', 'password')

vms = server.get_registered_vms()
for vm in vms:
	virtual_machine = server.get_vm_by_path(vm)
	print virtual_machine.get_property('name'), virtual_machine.get_property('ip_address')
{% endhighlight %}
