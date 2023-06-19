# Properties

The builtin `property` decorator essentially makes a method behave like an attribute. The `property.setter` decorator allows setting the attribute a new value (it is read-only without the setter). Both of these are enabled via a method call, allowing any sort of runtime evaluation. This is what other languages would use "getter" and "setter" methods for, but using Python properties we can do it while still making it look like a normal variable:
```python
class InvisibleGetterSetter:
    def __init__(self):
        self._private_attr = ""

    @property
    def my_attr(self):
        print("accessed my_attr")
        return self._private_attr

    @my_attr.setter
    def my_attr(self, new_value):
        print("modifying my_attr")
        self._private_attr = new_value

obj = InvisibleGetterSetter()
assert obj.my_attr == ""
obj.my_attr = "new value"
assert obj.my_attr == "new value"
```

### As retroactive encapsulation

Normally in Python we name private attributes with a prefixed underscore (this does not enforce users to stay away from these, but rather is a convention). Properties allow more flexibility when changing an implementation without changing behavior of existing attributes. Properties allow us to retroactively create getter and setter methods for attributes which were not "protected" behind underscored names or otherwise exposed and used by users:
```python
class Url:
    def __init__(self, address):
        self.address = f"https://{address}"

# User code
website = Url("ariel.ninja")
website.address += "/mousefox"
content = fetch(website.address)
```

The user code is using the `Url.address` attribute as if it's a public attribute, because we haven't explicitly indicated that it's private. Some time later we wish to support more protocols such as http and ftp, but not break any existing user code:
```python
class Url:
    def __init__(self, address, protocol="https"):
        self._protocol = protocol
        self._address = address

    @property
    def address(self):
        return f"{self._protocol}://{self._address}"

    @address.setter
    def address(self, new_value):
        self._protocol, self._address = new_value.split("://")
```

In this extremely simplified example, we can encapsulate attributes retroactively without changing the public API of a class. Since Python is an interpreted dynamic language (not just dynamically-typed) this means that name lookups at runtime can be indirect (and convoluted). In general, such practices should be used tastefully as it makes the code harder to reason about.

References:
- https://docs.python.org/3/library/functions.html#property
- https://docs.python.org/3/howto/descriptor.html
