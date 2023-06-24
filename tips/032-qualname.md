# Qualified Names

Some objects in Python have a `__name__` attribute, which is normally the name it was defined as:
```python
import sys as imported_sys
print(imported_sys.__name__)  # "sys"

def myfunc(): ...
indirect_reference = myfunc
print(indirect_reference.__name__)  # "myfunc"
```

However two nested objects may be defined with the same name, which makes introspective debugging more difficult. For this purpose, there exists a similar `__qualname__` attribute, which considers the fully qualified name:
```python
class BaseClass:
    def method(): ...

class MyClass(BaseClass):
    def method():
        def nested_func(): ...
        # "nested_func"
        print(nested_func.__name__)
        # "MyClass.method.<locals>.nested_func"
        print(nested_func.__qualname__)

# Both print "method"
print(BaseClass.method.__name__)
print(MyClass.method.__name__)

# "BaseClass.method"
print(BaseClass.method.__qualname__)
# "MyClass.method"
print(MyClass.method.__qualname__)
```

An important caveat to consider is that the qualified name does not include the module name, and so it is not enough to distinguish between similar names from different modules.

References:
https://peps.python.org/pep-3155/
