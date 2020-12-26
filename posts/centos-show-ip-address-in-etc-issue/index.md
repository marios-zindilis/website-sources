---
title: Display IP Address in /etc/issue on CentOS
description: How to display the IP address on CentOS by automatically populating /etc/issue
first-published: 2013-11-09
---

I have several CentOS virtual machines that I only fire up when I need 
to test something, so I don't give them static IPs. For my own 
convenience, I added the following lines in `/etc/rc.local`, which 
get the IP Address that was leased to the machine by the DHCP server, 
and change `/etc/issue` to display it:

{% highlight bash %}
export IPADDR=$(ifconfig eth1 | grep 'inet ' | cut -d ':' -f 2 | cut -d ' ' -f 1)
sed -i "s/IP Address:.*/IP Address: $IPADDR/" /etc/issue
{% endhighlight %}

This way, once the VM boots up and gets an IP from DHCP, that IP will 
be displayed in the hypervisor's console.
