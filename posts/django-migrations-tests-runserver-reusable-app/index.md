---
title: Migrations, tests and runserver in Django reusable apps
description: How to create a minimal manage.py file to make migrations, run tests and run a development server in a reusable Django app.
first-published: 2021-06-16
tags:
- Django
---

How do you create migration files, run unit tests and preview your Django app in the browser when developing a reusable
app that is not nested inside a Django project?

<!-- read more -->

When first developing a Django-based website, there is typically a Django project and one or more Django apps. The
Django project provides `settings.py` for configuring the web application, `wsgi.py` and `asgi.py` for deploying it,
project-level `urls.py` for routing, and, crucially, `manage.py` for all the management tasks, like creating migrations,
migrating, running unit tests, running management commands, running a development web server (`runserver`), etc.

When developing a [reusable Django app][1] though, there is no Django project wrapped around it, so there is no
`manage.py` to help with unit tests, migrations and the development server. Fortunately, you can create a variant of the
default `manage.py` that also includes all the required contents of the default `settings.py`. This will work for both
running unit tests with `./manage.py test` and with making migrations with `./manage.py makemigrations`.

Assuming your Django app is named `my_reusable_app`:

```python
#!/usr/bin/env python

import sys
sys.path.append('..')

from django.conf import settings
settings.configure(INSTALLED_APPS=['my_reusable_app'])

import django
django.setup()

from django.core.management import execute_from_command_line
execute_from_command_line(sys.argv)
```

The only setting required for unit tests and migrations is `INSTALLED_APPS`. You need to tell Python where to import the
app from, so assuming that you place this `manage.py` file in the same directory as the app code, you append `..` to the
system path.

If you also want to run the development server with `./manage.py runserver`, you need two additional settings: the
`ROOT_URLCONF` setting, and one of either `DEBUG=True` or a non-empty `ALLOWED_HOSTS`. Here's a complete example with
the latter option:

```python
#!/usr/bin/env python

import sys
sys.path.append('..')

from django.conf import settings
settings.configure(
    INSTALLED_APPS=['my_reusable_app'],
    ALLOWED_HOSTS=['localhost'],
    ROOT_URLCONF='urls',
)

import django
django.setup()

from django.core.management import execute_from_command_line
execute_from_command_line(sys.argv)
```

You could also place your `manage.py` file in the parent directory of your app, in which case you would need these two
changes:

```diff
-sys.path.append('..')

-    ROOT_URLCONF='urls',
+    ROOT_URLCONF='my_reusable_app.urls',
```

You don't need to append anything to the system path in this case, because the currect directory in included by default.

The utility of running the development server like this is reduced by the lack of a database. If you need a database for
development, you can add a `DATABASES` section in the configuration and run `./manage.py migrate`. I'll leave this one
as an exercise.

## Source Control and App Installation Artifacts ##

Should you include your custom `manage.py` in source control (e.g. Git), and should you include it in the installation
files that your users get?

I think you should share your custom `manage.py` in source control, so that other developers have an easy way to run
tests and make migrations for code they contribute to the app. It is not technically required, but it is convenient to
have it.

On the other hand, I think you should exclude this custom `manage.py` from the installable packages you distribute to
your users. There's no need to ship this file to customers, as it is only used for development, plus there may be some
unintended side effects (can't think of any right now, but you never know). If you use [setuptools][2] to package your
reusable Django app, you can add a `MANIFEST.in` file with a line like this:

```
exclude my_reusable_app/manage.py
```

That is, assuming `MANIFEST.in` is in the parent directory of the app code.

<hr>

Image credit: *Flowers Of Ireland*, by Krista Stith. In the Public Domain.
[Source](https://www.publicdomainpictures.net/en/view-image.php?image=181317&picture=flowers-of-ireland).

<!-- links -->
[1]: https://docs.djangoproject.com/en/3.2/intro/reusable-apps/ "How to write reusable apps"
[2]: https://pypi.org/project/setuptools/ "setuptools"
