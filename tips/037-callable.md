# Callable Objects

Python objects of a class defining a `__call__` method can be called like functions. Use the builtin `callable` function to determine if an object can be called.

The `functools` module defines the very useful `partial` and `partialmethod` which are ostensibly functions that return functions with preset arguments. We can demonstrate Python's expressiveness by reimplementing them:
```python
class partial:
    def __init__(self, func, /, *args, **kwargs):
        self._func = func
        self._args = args
        self._kwargs = kwargs

    def __call__(self, *args, **kwargs):
        return self._func(*self._args, *args, **(self._kwargs | kwargs))

credentials = partial(zip, ["name", "website"], strict=True)
assert callable(credentials)
my_intro = dict(credentials(["Ariel Horwitz", "https://ariel.ninja"]))
```

Note how we did not use the conventional CamelCase for our class name, instead using lowercase to indicate this is a function. In practice, the first call is in fact an instantiation of the class, and it returns an instance object that is indeed callable like a function. Python functions are themselves *objects* of a builtin ***function type***.

Along with [our overview of the Descriptor Protocol](/tips/036-descriptor-protocol.md), you are now armed to go in depth on the magic of bound methods, class methods, and static methods. Stay tuned!

References:
- https://docs.python.org/3/reference/datamodel.html
- https://docs.python.org/3/library/functions.html
- https://docs.python.org/3/library/functools.html#functools.partial
