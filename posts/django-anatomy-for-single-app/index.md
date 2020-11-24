---
title: Single-App Django Project Anatomy
description: How to structure the code in Django project that only contains one app.
first-published: 2017-01-06
last-modified: 2017-08-07
tags:
- Django
exports:
- ellak
- planet-sysadmin
---

With all the magic that comes packaged within Django, it's easy to forget that the code of a Django project is as
malleable as any other Python project's code. This post shows how to simplify the structure of a Django project, when
you will only ever have a single app.

<!-- read more -->

<div class="alert alert-info" role="alert">
<b>Edit 2017-08-07:</b> <a href="https://kalafut.net/">Jim Kalafut</a>
automated these steps in <a href="https://gist.github.com/kalafut/42bd31b2fdbf7a225da94e320d3e29ba">
a Bash script</a>.</div>

I watched a talk from DjangoCon 2014 called [Anatomy of a Django Project][1],
in which [Mark Lavin][2] examines the location of some of the files that are
automatically created by `django-admin startproject myproject` and by
`django-admin startapp myapp` (or `manage.py startapp myapp`, they are the
same thing). Two themes from that talk resonated with me:

1.  **Django projects are still Python code.** Many people who use Django,
    myself included, often forget that Django is still Python code. You can
    move files around if you want, you can rename them, you can do whatever
    you could do if your code was *not* a Django website, as long as modules
    are still able to import one another when needed. This article show you
    how to do something along those lines.

2.  **Naming the project and the app for single-app projects kinda sucks.**
    Sometimes you want to create a single-app Django project. Perhaps you are
    certain that your project will only ever contain one app, or maybe it is a
    proof-of-concept thing that you're tinkering with. You start a boilerplate
    project with `django-admin startproject something_awesome`, only to
    discover that you can't name your app `something_awesome` because that
    namespace is already occupied, in the form of a directory in which Django
    holds some project-wide files. You then end up naming your app `core`, or
    `common`, or `main`, or something that is not cool, and most importantly
    it's not semantic. This articles shows you how to overcome this hurdle.

  [1]: https://www.youtube.com/watch?v=ajEDo1semzs
    "DjangoCon 2014 - Anatomy of a Django Project"
  [2]: http://mlavin.org/ "Mark Lavin"

## Anatomy of a Default Django Project ##

Let's take a look at the file structure of a default Django project with one
app. To create one, do something like:

    django-admin startproject something_awesome
    cd something_awesome
    django-admin startapp core

Here's the file structure that Django created for you:

    $ tree
    .
    ├── core
    │   ├── admin.py
    │   ├── apps.py
    │   ├── __init__.py
    │   ├── migrations
    │   │   └── __init__.py
    │   ├── models.py
    │   ├── tests.py
    │   └── views.py
    ├── manage.py
    └── something_awesome
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py

You can see that the `something_awesome` directory is used to hold a few files,
those are project-wide files. You can't name your app `something_awesome`, so
you had to name it `core`.

## Simpler Structure for Single-App Projects ##

Here's how to create a simpler structure, if you're only creating a single-app
website.

1.  Create the Django project:

        django-admin startproject something_awesome
        cd something_awesome

    This is the file structure after that command:

        $ tree
        .
        ├── manage.py
        └── something_awesome
            ├── __init__.py
            ├── settings.py
            ├── urls.py
            └── wsgi.py

2.  Move everything from the `something_awesome` directory to the project root,
    then remove that directory and `__init__.py`:

        mv something_awesome/* .
        rmdir something_awesome
        rm __init__.py

    Here's the new structure:

        $ tree
        .
        ├── manage.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py

3.  Fix the location of `settings` in `manage.py`. Here's the diff: 

    ```diff
    --- manage.py-orig	2017-01-05 07:02:21.078737918 +0000
    +++ manage.py	2017-01-05 07:02:35.314857268 +0000
    @@ -3,7 +3,7 @@
     import sys
     
     if __name__ == "__main__":
    -    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "something_awesome.settings")
    +    os.environ.setdefault("DJANGO_SETTINGS_MODULE", "settings")
         try:
             from django.core.management import execute_from_command_line
         except ImportError:
    ```

4.  Fix `settings.py`. Here's the diff:

    ```diff
    --- settings.py-orig	2017-01-06 01:06:54.082362856 +0000
    +++ settings.py	2017-01-06 01:07:27.427015777 +0000
    @@ -49,7 +49,7 @@
         'django.middleware.clickjacking.XFrameOptionsMiddleware',
     ]
     
    -ROOT_URLCONF = 'something_awesome.urls'
    +ROOT_URLCONF = 'urls'
     
     TEMPLATES = [
         {
    @@ -67,7 +67,7 @@
         },
     ]
     
    -WSGI_APPLICATION = 'something_awesome.wsgi.application'
    +WSGI_APPLICATION = 'wsgi.application'
    ```

    If you are using SQLite as the database, then by default the location of
    the `sqlite` file will be in the parent directory of `settings.py`. Now
    that `settings.py` is in the root directory of the project, you don't want
    that anymore, you want the database file to be in the same directory as
    `settings.py`. Change the definition of the `BASE_DIR` constant in your
    `settings.py` from the default:

        BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

    ...to:

        BASE_DIR = os.path.dirname(os.path.abspath(__file__))

5.  Fix `wsgi.py`. Here's the diff:

    ```diff
    --- wsgi.py-orig	2017-01-06 01:09:38.145285387 +0000
    +++ wsgi.py	2017-01-06 01:09:56.393573270 +0000
    @@ -11,6 +11,6 @@
     
     from django.core.wsgi import get_wsgi_application
     
    -os.environ.setdefault("DJANGO_SETTINGS_MODULE", "something_awesome.settings")
    +os.environ.setdefault("DJANGO_SETTINGS_MODULE", "settings")
    ```

6.  At this point you have a Django project with a *flat* file structure, with
    no directory occupying the cool name that you came up with. You can now
    create an app called whatever you want, e.g:

        ./manage.py startapp something_awesome

    Here's the new structure:

        $ tree
        .
        ├── manage.py
        ├── settings.py
        ├── something_awesome
        │   ├── admin.py
        │   ├── apps.py
        │   ├── __init__.py
        │   ├── migrations
        │   │   └── __init__.py
        │   ├── models.py
        │   ├── tests.py
        │   └── views.py
        ├── urls.py
        └── wsgi.py

You can now get on with developing your application in this simpler setup.

## Final Thoughts ##

As I mentioned in the beginning, never forget that Django is just Python code.
You, as the developer, have the power to manipulate it however you want. If you
are interested in this matter, check out the DjangoCon 
[Anatomy of a Django Project][1] talk on Youtube.
