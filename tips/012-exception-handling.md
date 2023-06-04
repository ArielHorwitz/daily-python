## Exception Handling

Exceptions are events that occur when the program encounters an error, and are used to gracefully handle the errors and/or propagate them up the call stack.

When an exception occurs in a `try` block and it matches one of the `except` cases, the exception will be caught (not propagated) and that exception block will be executed:

```python
def load_config() -> str:
    try:
        with open(CONFIG_FILE) as config_file:
            return config_file.read()
    except (FileNotFoundError, UnicodeError):
        # Missing or corrupted config file
        fix_config_file()
        return DEFAULT_CONFIG
```

Exceptions are raised with the `raise`keyword:

```python
def silly_division(num, denom):
    if denom % 3 == 0:
        raise ArithmeticError("Denominator cannot be divisible by 3")
    return num / denom
```

As your logic becomes more complex, it is recommended to define your own exceptions. The `from` keyword allows you to chain exceptions, allowing you to indicate that one error is the result of another. This allows code to make errors much more understandable for users that are not familiar with the implementation details.

```python
"""A library for the Deep Thought Supercomputer."""
from traceback import format_exc

class DeepThoughtError(Exception):
    """Custom error for exceptions from this library."""
    pass

def answer_life_universe_everything():
    try:
        # Very high level code
        return ...
    except ArithmeticError as e:
        # Let's "hide" implementation details and present a
        # more user-friendly error
        raise DeepThoughtError(
            "Calculation error, "
            "did you forget to call gain_omniscient_perspective "
            "before calling answer_life_universe_everything?"
        ) from e
    except Exception as e:
        # Some unfamiliar exception, this should be reported as a bug
        log(format_exc())
        raise DeepThoughtError(
            "There was an error calculating the answer to life, the "
            "universe, and everything. Please report this bug."
        ) from e
    finally:
        # Some files need to be cleaned up whether it worked or not
        clean_up_deep_thought()
```

Use the base class `Exception` to catch any exception raised. After either or both try and except blocks are executed, the `finally` block will be executed (even if another exception is raised during handling).

References:
- https://docs.python.org/3/tutorial/errors.html
- https://docs.python.org/3/library/exceptions.html
- https://peps.python.org/pep-3109/
- https://peps.python.org/pep-3134/
