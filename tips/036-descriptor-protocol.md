# The Descriptor Protocol

The Descriptor Protocol in Python enables customizing attribute access. We can customize retrieving, assigning, and deleting attributes via dot notation by assigning the attribute name in the object's class to a *descriptor* (an object that defines `__get__`, `__set__`, or `__delete__`). This enables flexible and expressive syntax when using dot notation.

Like the builtin `property`, we can use this to enforce or manipulate attribute computation and assignment while maintaining backward compatibility. Let's reimplement the builtin `property` descriptor and it's corresponding decorator:
```python
class Descriptor:
    def __init__(self, fget, fset=None):
        self._fget = fget
        self._fset = fset or self._raise_readonly

    def __get__(self, instance, owner=None):
        return self._fget(instance)

    def __set__(self, instance, value):
        self._fset(instance, value)

    def _raise_readonly(self, instance, value):
        attr_name = f"{instance.__class__.__qualname__}.{self._fget.__name__}"
        raise AttributeError(f"`{attr_name}` is read-only")

    def setter(self, func):
        self._fset = func
        return self

def prop(func):
    # Decorator for a property-like descriptor
    return Descriptor(func)
```

This behaves similarly to the builtin `property` decorator:
```python
class MyClass:
    def __init__(self):
        self._foo = None

    @prop
    def foo(self):
        return self._foo

    @foo.setter
    def foo(self, value):
        self._foo = value
```

The descriptor is assigned to the `cls.__dict__`, as opposed to `obj.__dict__`. Normally when looking up attributes using dot notation the object's `__dict__` takes priority over the type's `__dict__`, with an exception if the value in the latter is a descriptor (which prevents instances from overwriting these attributes).

References:
- https://docs.python.org/3/reference/datamodel.html#descriptor-invocation
- https://docs.python.org/3/howto/descriptor.html
