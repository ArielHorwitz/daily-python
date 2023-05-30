# The functools wraps decorator

Yesterday, we discussed decorators. We saw something interesting: when wrapping a function with a decorator we effectively replace the function. This makes sense, because we need to let the wrapper call the function itself so that it can modify the behavior. However it is not intuitive that we define a function with a name and docstring but end up getting a function with a different name and docstring:
```python
def mydecorator(func):
    def wrapper(*args, **kwargs):
        """Wrapping function."""
        return func(*args, **kwargs)
    return wrapper

@mydecorator
def myfunc():
    """Do stuff."""
    ...

# Prints "wrapper" instead of "myfunc":
print(myfunc.__name__)
# Prints "Wrapping function." instead of "Do stuff.":
print(myfunc.__doc__)
```

It is possible to get around this manually, however there exists a very convenient decorator in the `functools` module called `wraps` which solves our issue. If we use the following definition for `mydecorator` and rerun the first code snippet we find that the print statements are as we would intuitively expect:
```python
from functools import wraps

def mydecorator(func):
    @wraps(func)  # Add this to make `wrapper` look like `func`
    def wrapper(*args, **kwargs):
        """Wrapping function."""
        return func(*args, **kwargs)
    return wrapper
```
Note how decorators can also take arguments other than the function they wrap. In this case `wraps` needs to take `func` as an argument because we want to modify `wrapper`'s attributes to reflect those of `func`.

References:
https://docs.python.org/3/library/functools.html#functools.wraps
