## Annotations and Type Hinting

Python is a dynamically typed language: the same variable at any time may
point to any arbitrary data type, and the same function may take arguments
of any type. While this enables quick and easy development in general, it is
notorious for producing unreliable code.

Over the past several versions, Python has been providing more features for
annotations. These can be used for "type hints" as labels on variables and
parameters that indicate what data type it should be. However, Python
enforces almost nothing about these type hints.

"It should also be emphasized that Python will remain a dynamically typed
language, and the authors have no desire to ever make type hints mandatory,
even by convention."
-- PEP 484

This means we can pretty much put any expression we want as a "type hint":
```python
def available_for_work(_: ...): ...
def independent_and_results_driven(_: 420.69 = ...): ...
def hire_me(_: "check out my online profiles" = ...): ...
```

What does it even mean to have a parameter of type 420.69? Python has no
opinion on the matter, and wishes to allow the Python ecosystem figure it
out. A common example of this is how Python tools understand this:
```python
class HireMe:
    @classmethod
    def from_resume(cls, resume: str = "https://ariel.ninja") -> "HireMe":
        return HireMe()
```

While a `classmethod` can return an object of the type is it bound to, the
annotations on function signatures are evaluated at compile time. During
compilation at line 3, the name `HireMe` has not been defined yet. To
annotate this function definition, we must avoid using the real class name.
Instead we simply use a string as a type hint, and convention follows that
Python tools understand to associate a string literal annotation to yet-to-be
defined names.

References:
https://docs.python.org/3/library/typing.html
https://peps.python.org/pep-0484/
