# The NamedTuple

Python's `namedtuple` is a tuple data class with some additional features. Possibly their best use case is for ergonomically creating small data classes such as when combining multiple values to pass as arguments or return values. The recommended method of defining namedtuples since Python 3.6 is by subclassing `typing.NamedTuple`:
```python
from typing import NamedTuple

class Color(NamedTuple):
    r: float
    g: float
    b: float
    alpha: float = 1

yellow = Color(1, 1, 0)
magenta = Color(r=1, g=0, b=1)

print(yellow) # 'Color(r=1, g=1, b=0, alpha=1)'
print(yellow.b)
print(yellow[2])
r, g, b, a = magenta
for component in magenta:
    ...  # iterating: r, g, b, alpha
```

Instances of namedtuples are subclasses of tuple. They are immutable, hashable, iterable, unpackable, sortable, fast, small, and provide quality of life for debugging.

## Comparison with dataclasses
While the `dataclasses.dataclass` decorator probably provides all features of namedtuples, the namedtuple may offer a simpler way to achieve most of the common benefits desired by small datatypes. Dataclasses will be discussed in a future post.
