# Decorators

Python decorators are functions that are called with a function as an argument and return what will take their place. So for example:
```python
def decorator(func): ...

@decorator
def some_func(): ...
```

Is equivalent to:
```python
def decorator(func): ...
def some_func(): ...
some_func = decorator(some_func)
```

In effect, we have switched the the name `some_func` to refer to the *result* of calling `decorator` with `some_func` as an argument. Usually, the decorator returns a new function, which we expect has "wrapped" `some_func`. This can be used for setup and teardown, validation, and many other use cases. Let's take a look at this classic example:
```python
import random
import time

def measure(func):
    # Do not use this for performance testing
    def measure_wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(
            f"Call to {func=} "
            f"with {args=} {kwargs=} "
            f"elapsed in {elapsed*1000:.1f} ms"
        )
        return result
    return measure_wrapper

@measure
def suspect_func():
    time.sleep(random.random())

suspect_func()
print(suspect_func)
```

In this case, our decorator `measure` returns a function  `measure_wrapper` which will call the original function, while also measuring and printing how long it took. In the last line, we can see that the name `suspect_func` actually refers to the `measure_wrapper`, confirming we have "replaced" the functions.

But isn't it interesting that when we print `func` in `measure_wrapper` we see a reference to the original `suspect_func`? In a future post we will discuss scoping in Python, as well as a useful and relevant feature of functools.

References:
- https://docs.python.org/3/reference/compound_stmts.html#function
- https://peps.python.org/pep-0318/
- https://peps.python.org/pep-3129/
