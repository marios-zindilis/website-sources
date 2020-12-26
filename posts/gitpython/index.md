---
title: GitPython
description: Notes and links on GitPython
first-published: 2014-06-12
---

Installation
------------

The easy way is to use one of the utilities provided by Python's 
`setuptools`. If you don't have `setuptools`, you can install them with:

{% highlight bash %}
sudo apt-get install python-setuptools
{% endhighlight %}

...or with:

{% highlight bash %}
sudo yum install python-setuptools
{% endhighlight %}

... depending on your platform. You can then install GitPython with: 

{% highlight bash %}
sudo easy_install GitPython
{% endhighlight %}

After that, it will be available for you to import and use:

{% highlight python %}
import git
{% endhighlight %}

Usage
-----

### Initialize a Repository Object ###

If you have a local directory that has already been initialized as a 
Git repository (hint: it should contain a `.git` subdirectory), then 
you can simply create an object of it:

{% highlight python %}
import git
repo = git.Repo('/home/marios/repositories/test-repo')
{% endhighlight %}

See Also
--------

*   [Google Group for Discussions on GitPython](https://groups.google.com/forum/#!forum/git-python)
