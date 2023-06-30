# Dot Notation and Methods
> This post continues [The Descriptor Protocol](/tips/036-descriptor-protocol.md) and [Callable Objects](/tips/037-callable.md).

Did you know that Python will pass `self` to any function that is invoked using dot notation?

Even if it is defined outside the class and is assigned retroactively!
```python
def func(): ...
class MyClass: ...
MyClass.func = func

MyClass.func()  # No problem
obj = MyClass()
obj.func()  # TypeError: func() takes 0 positional arguments but 1 was given

# `obj` is passed as first argument (as `self`)
# Python?? How do you do this?
```

In most cases dot notation on an object seems quite simple: check if the attribute name exists in `obj.__dict__`, otherwise check `cls.__dict__` (and base classes) until found or raise an attribute error. This is how inheritence is most intuitively understood, though in practice attribute lookup is far more complicated.

We can learn about the interplay of dot notation and methods in Python by wrapping the builtin `function` in a descriptor. As we know from [our previous post](/tips/036-descriptor-protocol.md), the descriptor protocol dictates that the type and/or instance of the object on the left of the dot notation is passed to the descriptor on the right. This enables expressive and dynamic attribute lookup and we can use this to *bind functions to objects*, making them "methods":
```python
class Function:
    def __init__(self, func):
        self.__func__ = func

    def __call__(self, *args, **kwargs):
        # Normally just a thin wrapper
        return self.__func__(*args, **kwargs)

    def __get__(self, instance, owner=None):
        if instance is not None:
            # Bind to the object left of dot notation
            return BoundMethod(self.__func__, instance)
        return self

class BoundMethod:
    # Alternative to `functools.partial`
    def __init__(self, func, owner):
        self.__func__ = func
        self.__self__ = owner

    def __call__(self, *args, **kwargs):
        return self.__func__(self.__self__, *args, **kwargs)
```

While not a true analogue, the above `Function` type is able to mimic the behavior of methods on objects:
```python
def func(self): ...
class MyClass: ...
MyClass.func = Function(func)

obj = MyClass()
obj.func()  # Passes `obj` as first arguement (as `self`)
```

*Curious how `classmethod` or `staticmethod` can modify or disable this behavior?* Stay tuned for the next post!
