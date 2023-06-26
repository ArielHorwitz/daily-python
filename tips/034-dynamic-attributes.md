# Dynamic Attributes in Python

Python's dynamic nature allows us to easily access and manipulate object attributes flexibly. The `getattr`, `setattr`, and `hasattr` built-in functions are particularly useful for this:
```python
class MyNameSpace:
    pass
ns = MyNameSpace()
assert not hasattr(ns, "eggs")
setattr(ns, "eggs", "spam")
assert hasattr(ns, "eggs")
assert getattr(ns, "eggs") == ns.eggs == "spam"
```

The `__getattr__` and `__setattr__` special methods allow us to get and set attributes dynamically:
```python
class Calculator:
    _vars = {}
    def __getattr__(self, name):
        if name.startswith("add_"):
            _, left, right = name.split("_")
            left = float(self._vars.get(left, left))
            right = float(self._vars.get(right, right))
            return left + right
        return super().__getattr__(name)

    def __setattr__(self, name, value):
        if name.startswith("var_"):
            var_name = name.removeprefix("var_")
            self._vars[var_name] = value
        return super().__setattr__(name, value)

calc = Calculator()
calc.var_fine = 137
assert calc.add_420_69 == 489
assert calc.add_fine_3 == 140
```

This allows us to create objects that behave in more expressive and adaptable ways. However, while powerful, dynamic attribute access should be used wisely and sparingly, as overuse can lead to code that is harder to maintain and debug.

In particular, it's crucial to be aware of the risk of recursion when defining `__getattr__` or `__setattr__`. If we try to access or set an attribute inside the respective methods, it may lead to infinite recursion since it would trigger the method again. To avoid recursion, we should use `super().__getattr__()` and `super().__setattr__()` or rely on methods that don't utilize the custom attribute access behavior.

References:
- https://docs.python.org/3/library/functions.html
- https://docs.python.org/3/reference/datamodel.html
