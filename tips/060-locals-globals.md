# Local And Global Namespaces

Yesterday we discussed finding names in scopes. Today we'll look at these namespaces more directly using Python's powerful runtime introspection tools. The builtin `locals()` function returns the current namespace as a dictionary:
```python
def func():
    myvar = ...
    assert "myvar" in locals()
```

The namespace includes all the defined "names", which are the words used to identify objects in your code:
```python
def func(param):
    assert "param" in locals()

    import main
    assert "main" in locals()

    for i in range(1): ...
    assert "i" in locals()

    with open("main.py") as file: ...
    assert "file" in locals()

    def inner(): ...
    assert "inner" in locals()

    class MyClass: ...
    assert "MyClass" in locals()

    try:
        raise Exception()
    except Exception as err:
        assert "err" in locals()
```

The module-level namespace is also the globals namespace, which we can inspect using the builtin `globals()`:
```python
# main.py
GLOBAL_VAR = ...
assert "GLOBAL_VAR" in globals()
assert locals() is globals()

def func():
    assert locals() is not globals()
    assert "GLOBAL_VAR" in globals()
```

Namespaces are often already populated by some handy values. For example the globals (module-level) namespace includes the name of the module and file:
```python
assert globals()["__name__"] is __name__
assert globals()["__file__"] is __file__

assert __name__ == "__main__"
assert __file__.endswith("main.py")
```

References:
https://docs.python.org/3/library/functions.html#locals
