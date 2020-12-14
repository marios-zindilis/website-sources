---
title: Clone a remote repository with GitPython
Description: How to clone a remote repository using GitPython
first-published: 2015-02-08
---

Example code:

{% highlight python %}
import git

remote = 'https://github.com/gitpython-developers/GitPython.git'
local = '/home/marios/Tests/gitpython'

git.Repo.clone_from(remote, local)
{% endhighlight %}
