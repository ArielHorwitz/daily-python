## Context Managers

Context managers enforce control flow for resources that need to be properly acquired and released, such as files and network connections. When using the `with` statement, the context manager's `__enter__()` method is called before the contextualized block, and the `__exit__()` method is called after even if an exception is raised. The most commonly used context manager in Python may be opening files:
```python
# File is opened before this block starts
# and closed after this block ends
with open("available-for-hire.txt", "r") as file:
    resume = file.read()

# No worry that the file may still be open,
# which can otherwise cause many problems
print(resume)
```

Suppose we are dealing with a feature that requires some environment variable to be set in order to behave properly. But changing an environment variable can cause many problems. Bar redesign, we *must* restore the environment variable to it's original state when we finish this task - sounds perfect for context managers. We will avoid the verbose method to define a context manager and instead we'll use the `contextlib.contextmanager` decorator:
```python
from contextlib import contextmanager

@contextmanager
def temporary_path(path):
    try:
        original_path = get_path()
        set_path(path)
        yield  # Run contextualized block
    finally:
        set_path(original_path)

def frobnicate():
    # get_path() == "/src"
    with temporary_path("/some/path"):
        # get_path() == "/some/path"
        ...
        # get_path() == "/some/path"
    # get_path() == "/src"

def exceptional_code():
    # get_path() == "/src"
    try:
        with temporary_path("/other/path"):
            # get_path() == "/other/path"
            raise Exception("I crashed")
    except Exception:
        ...
    # get_path() == "/src"
```

Browse the references to learn about exception handling, async, and much more in the `contextlib` module.

References:
- https://docs.python.org/3/library/contextlib.html
- https://docs.python.org/3/reference/compound_stmts.html#with
- https://peps.python.org/pep-0343/
