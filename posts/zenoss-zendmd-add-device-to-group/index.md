---
title: "Zenoss: Add a device to a Group with ZenDMD"
first-published: 2014-02-19
Last Updated: 2016-11-06
tags:
- Zenoss
---

As user `zenoss` (or whatever user your Zenoss application runs as), run 
`zendmd`, and:

{% highlight python %}
Device = find('My Awesome Server')
Device.addDeviceGroup('My Wonderful Group')
commit()
{% endhighlight %}

This will make the device named `My Awesome Server` a member of the group named 
`My Wonderful Group`. 
