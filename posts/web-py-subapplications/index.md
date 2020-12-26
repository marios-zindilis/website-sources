---
title: web.py Subapplications
description: Notes and links on web.py subapplications
first-published: 2014-04-22
---

To better organize your source code in a web.py application, you can break it 
down in subapplications. Those subapplications will be imported as Python 
modules in your main web.py script (which, in the below examples is 
`index.py`).

Troubleshooting
---------------

### `AttributeError: 'module' object has no attiribute ...` ###

If you get the error:

    AttributeError: 'module' object has no attribute 'app'

...then you might be running into Python naming collissions. Check if you have 
given one of your subapplications the same name as an existing Python module. 
For example, you shouldn't name a subapplication `user`, since that is an 
existing Python module.

### `ImportError: No module named ...` ###

If you get the error:

    ImportError: No module named subapp

... (where `subapp` is the name of your subapplication), then Python cannot 
`import` the subapplication as a module. As a first step, create an empty file 
named `__init__.py` in the same directory as your subapplication, and try 
again. If you still get that error, and assuming your subapplications are in 
the same directory as your main application, you can try this ugly hack in the 
very beginning of your main application script:

```python
import inspect
import os
import sys
sys.path.append(os.path.dirname(os.path.abspath(inspect.getfile(inspect.currentframe()))))
```
