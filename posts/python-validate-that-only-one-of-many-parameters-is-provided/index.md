---
title: Validate that only one of many parameters was provided in Python
description: A simple pattern for validating that out of many parameters only one was provided
first-published: 2020-12-29
tags:
- Python
---

Sometimes, a function or class constructor can take many possible parameters, but only one parameter can have a value
at any one time, excluding all others. One way to validate this is included in the Python standard library. Copying
from the `uuid.py` module:

```python
class UUID:
    def __init__(self, hex=None, bytes=None, bytes_le=None, fields=None,
                       int=None, version=None,
                       *, is_safe=SafeUUID.unknown):
        if [hex, bytes, bytes_le, fields, int].count(None) != 4:
            raise TypeError('one of the hex, bytes, bytes_le, fields, '
                            'or int arguments must be given')
```

The idea here is that since all **five** mutually exclusive parameters `hex`, `bytes`, `bytes_le`, `fields` and `int`
have a default value of `None`, when one of them is specified during instatiation, there will then be **four**
parameters left with value `None`.

Image credit: *Wicklow Mountains*, by Marcus Tan. In the Public Domain.
[Source](https://www.publicdomainpictures.net/en/view-image.php?image=20993&picture=wicklow-mountains).
