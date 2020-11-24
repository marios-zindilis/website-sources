---
title: Back-end Validation for Django Model Field Choices
desciption: How to validate that a field value is within a list of choices in Django
first-published: 2017-05-04
tags:
- django
exports:
- ellak
- planet-sysadmin
---

In Django, you can provide a list of values that a field can have when creating a model. This post shows a way to
validate the values passed which creating a new instance or saving an existing one.

<!-- read more -->

For example, here's a minimalistic model for storing musicians:

```python
from django.db import models


class Artist(models.Model):

    TYPE_CHOICES = (
        ('Person', 'Person'),
        ('Group', 'Group'),
        ('Other', 'Other'),)

    name = models.CharField(max_length=100)
    type = models.CharField(max_length=20, choices=TYPE_CHOICES)
```

Using this code in the Admin interface, as well as in Forms, will make the
"Type" field be represented as a drop-down box. Therefore, if all input comes
from Admin and Forms, the value of "Type" is *expected* to be one of those
defined in the `TYPE_CHOICES` tuple. There are 2 things worth noting:

1.  Using `choices` is a presentation convenience. There is no restriction of
    the value that can be sent to the database, even coming from the front-end.
    For example, if you use the browser's *inspect* feature, you can edit the
    drop-down list, change one of the values and submit it. The back-end view
    will then happily consume it and save it to the database.

2.  There is no validation at any stage of the input lifecycle. You may have a
    specified list of values in the Admin or Forms front-end, but your
    application  may accept input from other sources as well, such as
    [management commands][1], or [bulk imports from fixtures][2]. In none of
    those cases will a value that is not in the `TYPE_CHOICES` structure be
    rejected.

  [1]: https://docs.djangoproject.com/en/1.11/howto/custom-management-commands/
    "Writing custom django-admin commands"
  [2]: https://docs.djangoproject.com/en/1.11/howto/initial-data/
    "Providing initial data for models"

## Validation ##

So let's find a way to fix this by adding some validation in the back-end. 

Django has this amazing feature called [signals][3]. They allow you to attach
extra functionality to some actions, by emitting a signal from that action. For
example, in this case we will use the `pre_save` signal, which gets executed
just before a model instance is saved to the database.

Here's the example model, with the signal connected to it:

```python
from django.db import models


class Artist(models.Model):

    TYPE_CHOICES = (
        ('Person', 'Person'),
        ('Group', 'Group'),
        ('Other', 'Other'),)

    name = models.CharField(max_length=100)
    type = models.CharField(max_length=20, choices=TYPE_CHOICES)


models.signals.pre_save.connect(validate_artist_name_choice, sender=Artist)
```

That last line will call a function named `validate_artist_name_choice` just
before an Artist is saved in the database. Let's write that function:

```python

def validate_artist_name_choice(sender, instance, **kwargs):
    valid_types = [t[0] for t in sender.TYPE_CHOICES]
    if instance.type not in valid_types:
        from django.core.exceptions import ValidationError
        raise ValidationError(
            'Artist Type "{}" is not one of the permitted values: {}'.format(
                instance.type,
               ', '.join(valid_types)))
```

This function can be anywhere in your code, as long as you can import it in the
model. Personally, if a `pre_save` function is specific to one model, I like to
put it in the same module as the model itself, but that's just my preference.

So let's try to create a couple of Artists and see what happens. In this
example, my App is called "v".

```python
>>> from v.models import Artist
>>> 
>>> some_artist = Artist(name='Some Artist', type='Person')
>>> some_artist.save()
>>> 
>>> some_other_artist = Artist(name='Some Other Artist', type='Alien Lifeform')
>>> some_other_artist.save()
Traceback (most recent call last):
  File "/usr/lib/python3.5/code.py", line 91, in runcode
    exec(code, self.locals)
  File "<console>", line 1, in <module>
  File "/home/vs/.local/lib/python3.5/site-packages/django/db/models/base.py", line 796, in save
    force_update=force_update, update_fields=update_fields)
  File "/home/vs/.local/lib/python3.5/site-packages/django/db/models/base.py", line 820, in save_base
    update_fields=update_fields)
  File "/home/vs/.local/lib/python3.5/site-packages/django/dispatch/dispatcher.py", line 191, in send
    response = receiver(signal=self, sender=sender, **named)
  File "/home/vs/Tests/validator/v/models.py", line 14, in validate_artist_name_choice
    ', '.join(valid_types)))
django.core.exceptions.ValidationError: ['Artist Type "Alien Lifeform" is not one of the permitted values: Person, Group, Other']
```

Brilliant! We now have a expressive exception that we can catch in our views or
scripts or whatever else we want to use to create model instances. This
exception is thrown either when we call the `.save()` method of a model
instance, or when we use the `.create()` method of a model class. Using the
example model, we would get the same exception if we ran:

```python
Artist.objects.create(name='Some Other Artist', type='Alien Lifeform')
```

Or even:

```python
Artist.objects.get_or_create(name='Some Other Artist', type='Alien Lifeform')
```

  [3]: https://docs.djangoproject.com/en/1.11/topics/signals/ "Django Signals"

## Test It! ##

Here's a test that triggers the exception:

```python
from django.test import TestCase
from django.core.exceptions import ValidationError
from .models import Artist


class TestArtist(TestCase):
    def setUp(self):
        self.test_artist = Artist(name='Some Artist', type='Some Type')

    def test_artist_type_choice(self):
        with self.assertRaises(ValidationError):
            self.test_artist.save()
```

## Conclusion ##

Using Django's `pre_save` functionality, you can validate your application's
input in the back-end, and avoid any surprises. Go forth and validate!

