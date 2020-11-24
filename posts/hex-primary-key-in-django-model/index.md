---
title: Hexadecimal Primary Key in Django Model
description: How to get a hexadecimal ID primary key in a Django model
first-published: 2015-12-06
tags:
- Django
---

The following snippet of code will make the primary key in a Django model to be a hexadecimal string of 8 characters
instead of an integer.

<!-- read more -->

I wrote this earlier today, and it works fine, but I have a bad feeling about
it. I don't know why yet, but something doesn't feel right. Nevertheless, I am
putting it here as a note to myself. You should probably not use it. If you do,
note that if you exhaust the possible IDs, `generate_id()` will recurse 
forever.

```python
import os
from binascii import hexlify
from django.db import models

# Create your models here.
class Person(models.Model):
    '''Hold a Person object'''

    def generate_id():
        '''Generate an 8-character long hexadecimal ID'''
        possible = hexlify(os.urandom(4))
        try:
            Person.objects.get(id=possible)
        except Person.DoesNotExist:
            return possible
        else:
            return self.generate_id()

    id = models.CharField(
        max_length 8,
        primary_key=True,
        editable=False,
        default=generate_id
    )
    first_name = models.CharField(max_length=240)
```
