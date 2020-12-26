---
title: Contribute to an open source project on GitHub
description: A guide on how-to contribute to an open source project on GitHub
first-published: 2014-01-29
last-modified: 2014-07-22
---

This document will guide you through the steps required to contribute 
some source code fix or enhancement to an open source project hosted 
on [GitHub](https://github.com/). It is a bottom-up tutorial, starting 
from signing up to GitHub.

As a real life example for this guide, we will make a contribution to 
a not-so-typical GitHub project, the [Free Programming Books](https://github.com/vhf/free-programming-books) 
list. This project in GitHub is the source of content of the webpage 
[List of Free Programming Books](http://resrc.io/list/10/list-of-free-programming-books/). 

Sign Up to GitHub
-----------------

First, you need to have an account on GitHub. There are non-free 
accounts for non-open-source projects, but for the purpose of this 
guide, and for contributing to Open Source software, you only need a 
free account. 

Head over to [github.com](https://github.com/) and Sign Up!

Fork a Project
--------------

In order to get the code of a project, edit it and send it back to the 
developers, you need to create a **fork** of the source code. For the 
purpose of this example, let's assume that your username is 
"ChuckNorris" (meh... as if Chuck Norris needs to fork and edit source 
code... the source code fixes itself to avoid being roundhouse 
kicked...).

Head over to the page where the project you are interested in is 
hosted. For our example, go to 
[github.com/vhf/free-programming-books](https://github.com/vhf/free-programming-books/), 
and click on the **Fork** button. GitHub will take a few seconds, and 
will create a copy of the original repository under your account, in 
our example it would be `github.com/ChuckNorris/free-programming-books`.

Install Git
-----------

You will need <code>git</code> installed on your PC/workstation to 
download code from GitHub. On an Ubuntu (or other Debian-based) system, 
do:

    sudo apt-get install git

On a CentOS or other RedHat-based system:

    yum install git

All of the rest of these instructions in this guide are the same on 
Ubuntu (or similar) and CentOS (or similar) systems, although Ubuntu 
typically comes with newer version of packages, and might have better 
behaviour with Git operations. After you have installed Git, you should 
set your username and email address. These will be submitted with the 
changes that you will make, to the original repository:

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

Download the source code
------------------------

To download the source code locally on your PC/workstation, from 
**your** GitHub repository:

    git clone https://github.com/ChuckNorris/free-programming-books.git

This will create a directory named after the project (in our example 
"free-programming-books"), and put the source code files inside. You can 
now browse the source code files locally, on your workstation.

Connect the local directory with the original GitHub repository
---------------------------------------------------------------

Now that you downloaded the source code from your own repository, `git` 
knows where to push the changes that you will make. In `git` parlance, 
your repository is the **origin**.

You will most probably need to also create a connection to the original 
GitHub repository, so that Git knows where to pull updates from. In 
other words, if the developers of the original project do some changes, 
you should be able to update your directory on your workstation with 
those updates, so that you always work on the most current version of 
the code. In Git parlance, the original repository is the **upstream**.

To add the upstream repository:

    cd free-programming-books
    git remote add upstream https://github.com/vhf/free-programming-books.git 

Now, every time you want to work on the project, you can get an updated 
version of it by running:

    git fetch upstream

This will **not** change any files that you already edited locally, 
unless you also do:

    git merge upstream/master

Edit source files and push changes
----------------------------------

At this point, you should have a copy of the source code that you want 
to edit on your GitHub repository, the one that you **forked** from the 
**upstream** repository, and is now your **origin**. You should also 
have a **local** copy of the source code on a directory on your 
workstation.

You probably now want to edit one or more of the source code files, 
using the text editor of your preference. For the sake of this example, 
we will do a couple of changes in the file named `free-programming-books.md`, 
and then push them to the **origin**. The changes that we will do are:

1.  Add two programming books for the Picolisp language, and
2.  Add the Picolisp language to the table of contents.

For the record, here is the `diff` of the file before and after the changes:

    [marios@j free-programming-books]$ diff -u free-programming-books.md-original free-programming-books.md
    --- free-programming-books.md-original	2014-01-29 13:10:05.502982051 +0200
    +++ free-programming-books.md	2014-01-29 13:06:00.659982149 +0200
    @@ -108,6 +108,7 @@
     * [PC-BSD](#pc-bsd)
     * [Perl](#perl)
     * [PHP](#php)
    +* [PicoLisp](#picolisp)
     * [PostgreSQL](#postgresql)
     * [PowerShell](#powershell)
     * [Processing](#processing)
    @@ -1250,6 +1251,11 @@
     * [PHP 5 Power Programming](http://www.informit.com/content/images/013147149X/downloads/013147149X_book.pdf)
     
     
    +###PicoLisp
    +* [PicoLisp by Example](http://www.scribd.com/doc/103733857/PicoLisp-by-Example)
    +* [PicoLisp Works](http://www.scribd.com/doc/103732688/PicoLisp-Works)
    +
    +
     ###PostgreSQL
     * [Practical PostgreSQL](http://www.commandprompt.com/ppbook/)

You can get a result similar to the above with `git diff`.

After you edit the source code file that you want, and save it on your 
workstation, it's time to push the changed file to your own GitHub 
repository, the **origin**. First, you **commit** your change to Git. 
This only affects files locally on your workstation. In this example, 
the file name is <code>free-programming-books.md</code>:

    git commit free-programming-books.md -m 'Added two books on PicoLisp'
    git push origin master

The `commit` command "gave" the changed file to Git to handle, with a 
short comment given after the <code>-m</code> option, and then the 
`push` command asked Git to push the committed file to GitHub, to the 
**master** branch of the **origin** repository. You will be asked for 
your username and password to GitHub during the **push**.

You can visit your GitHub repository now, and you will be able to see 
that the file you edited and pushed has changed.

Ask the original developers to pull your changes
------------------------------------------------

After you have edited one or more files, committed them to Git and 
pushed them to your own repository on GitHub, you will probably want to 
ask the developers of the original source code, the one that resides in 
the **upstream** to review your changes, and merge them into the 
project, if applicable.

This action is called a **pull request**, and is clearly documented at 
[Using Pull Requests](https://help.github.com/articles/using-pull-requests) 
on github.com. For the record, and for the completion of the real-world 
example used in this guide, the result was [Pull Request 672](https://github.com/vhf/free-programming-books/pull/672) 
on the **Upstream** repository.

Conclusion
----------

The procedure outlined above might seem too much at first, but after a 
couple of times, you will realize that it's actually pretty simple. If 
you were to count the commands used here, you would only have a 
handful. So, go ahead, become an open source developer!
