# The \_\_init\_\_ Module

We can subpackage modules by simple creating a folder with Python files next to the script we are running (usually `main.py`). In this case we add a folder called `mylib` with a file called `mymod.py`:
```python
# mylib/mymod.py
def main_func(message):
    print(message)
```

Which allows us to import it in the main script using dot notation:
```python
# main.py
from mylib.mymod import main_func

main_func("https://ariel.ninja")

# What are these objects?
import mylib
from mylib import mymod
print(mylib) # <module 'mylib' (<_frozen_importlib_external.NamespaceLoader object ...>)>
print(mymod) # <module 'mylib.mymod' from 'mylib/mymod.py'>
```

Those print statements at the end hint to our next step. `mylib` is the "package" that includes all the submodules. And `mymod` is a `module` object. But in fact `mylib` could be its own module, the top-level module of the package by containing an `__init__.py` file. If so, that is what gets imported when importing a library or package:
```python
# mylib/__init__.py
from .mymod import main_func
```

Notice how we can use relative paths by starting with a period like `from .mymod import ...`. Now we can use this init module by importing the package directly:
```python
# main.py
from mylib import main_func
main_func("Hire me!")

import mylib as ml
assert ml.__file__.endswith("__init__.py")

print(ml)  # <module 'mylib.mymod' from 'mylib/__init__.py'>
```
