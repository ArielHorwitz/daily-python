## Function signatures

You can use `/` (slash) or `*` (asterisk) to denote arguments in a function
signature. Arguments before the slash must be passed as positional, while
those after the asterisk must be passed as keywords.

```python
def func(positional_only, /, positional_or_keyword, *, keyword_only):
    ...

# Raises TypeError (got some positional-only arguments passed as keyword arguments)
func(positional_only=None)

# Raises TypeError (takes 2 positional arguments but 3 were given)
func(None, None, None)

# Allowed
func(None, None, keyword_only=None)

# Allowed
func(None, positional_or_keyword=None, keyword_only=None)
```

References:
https://peps.python.org/pep-0570/
