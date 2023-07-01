# Classmethods and Staticmethods
> This post continues [Dot Notation and Methods](/tips/038-dot-notation-methods.md).

In yesterday's post, we learned how Python binds functions to objects to make them methods. Functions are actually descriptors that will pass the object instance to the given function when invoked via dot notation (automatically passing `self`). Let's expand on yesterday's code and reimplement the builtin `staticmethod` and `classmethod` decorators.

For class methods, we pass the type of the instance which we get via the descriptor protocol as the first argument:
```python
class ClassMethod:
    def __init__(self, func):
        self.__func__ = func

    def __get__(self, instance, owner=None):
        instance = owner if instance is None else type(instance)
        return BoundMethod(self.__func__, instance)

def my_classmethod(func):  # Class method decorator
    return ClassMethod(func)
```

For static methods, we simply use the indirection and call the function without injecting arguments:
```python
class StaticMethod:
    def __init__(self, func):
        self.__func__ = func

    def __call__(self, *args, **kwargs):
        return self.__func__(*args, **kwargs)

    def __get__(self, instance, owner=None):
        return self.__func__

def my_staticmethod(func):  # Static method decorator
    return StaticMethod(func)
```

These let us use similar grammar:
```python
class MyClass:
    @my_classmethod
    def class_method(cls): ...

    @my_staticmethod
    def static_method(): ...

# Equivalent to:
class MyClass:
    def class_method(cls): ...
    def static_method(): ...

    class_method = ClassMethod(class_method)
    static_method = StaticMethod(static_method)

MyClass.static_method()  # No arguments passed
# Class passed as first argument:
MyClass.class_method()
MyClass().class_method()
```

Here's a mystery: how come the indirection works to prevent the instance of `StaticMethod` to be passed as the first argument? After all, the builtin `function` type is a descriptor, and we are looking it up using dot notation: `self.__func__`. I actually don't know the answer. I checked if it has to do with the attribute name matching the function name, but it doesn't seem to be that. Remind me to check out the source code for the builtin `function` type.
