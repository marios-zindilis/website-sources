---
title: Bash Special Variables
description: Notes on special variables in Bash scripts
first-published: 2013-12-25
last-modified: 2014-09-29
---

<p class="lead">
  A couple of notes on special variables available in the Bash shell.
</p>

`$0`: Name of the script
------------------------

The special variable `$0` contains the name of the script. It is useful when 
returning usage instructions to the user that run the script. For example:

{% highlight bash %}
echo "Usage: $0 --option-1 --option-2";
{% endhighlight %}

`$#`: Number of arguments
-------------------------

The special variable `$#` contains the number of arguments passed to the 
script. If no arguments are passed, it is equal to 0. It is useful to examine 
the value of this variable before taking further actions, if a script *must* 
be run with a minimum number of arguments. 

Here is an example of `$#` used together with `$0`:

{% highlight bash %}
if [ $# -eq 0 ]; then
    echo "Insufficient number of arguments!";
    echo "Usage: $0 /path/to/file";
fi
{% endhighlight %}

`$?`: Most recent exit code
---------------------------

The special variable `$?` contains the exit code of the most recently 
executed command. It is useful to examine this to determine whether or 
not the previous command completed succesfully or returned an error. 

Typically, commands that complete succesfully return an exit code equal 
to 0, with other values indicating different errors, but check each 
command's documentation to verify this.

In the following example, the existense of the word "test" in a file 
is checked:

{% highlight bash %}
grep test /path/to/file > /dev/null;
if [ $? -eq 0 ]; then
    echo "/path/to/file contains the word 'test'";
else
    echo "/path/to/file does not contain the word 'test'";
fi
{% endhighlight %}
