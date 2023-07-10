# Numpy Views And Copies
> This post is part of a series on numpy. See [earlier posts](/).

The ndarray object doesn't contain data directly, instead it only points to the address in memory where the data is stored. This means that two ndarray objects can share data. When creating an ndarray from another ndarray, you may sometimes get a *view* and sometimes get a *copy*. We can confirm this using the `base` property:
```python
import numpy as np
arr = np.arange(10)
assert arr.base is None   # Not a view

view = arr.view()         # Create a view to same data
assert view.base is arr   # Is view of `arr`
arr[0] = 69               # As if modified `arr` and `view`
assert np.array_equal(view, arr)

copy = arr.copy()         # Create a copy with new data
assert copy.base is None  # Not a view
copy[0] = 420             # Modified `copy` only
assert not np.array_equal(copy, arr)
```

In this example we are explicitly creating a view and a copy, however sometimes it is not so obvious. Without getting into too much details of why and how, a general rule of thumb is that when slicing an array we get a view [^1] . We should check the documentation for results of functions and methods.
```python
arr = np.arange(32)
view = arr.reshape(8, 2, 2)[5:2:-1]
assert view.base is arr
view += 1
assert view.base is arr
assert view.view().view().base is arr
assert view.view(float).base is arr
assert view.astype(float).base is None

new = view + 1
assert new.base is None
flat = new.reshape(-1)
assert flat.base is new
flattened = new.flatten()
assert flattened.base is None
```

You might already suspect that copying all of the array's data is slower than creating a new ndarray object that simply shares the same data. Using views improves performance and memory use, but can also cause bugs if used incorrectly. While this tradeoff is not unique to numpy, it is important to keep in mind numpy's unique view/copy behavior.

References:
- https://numpy.org/doc/stable/user/basics.copies.html
- https://numpy.org/doc/stable/reference/generated/numpy.ndarray.view.html
- https://numpy.org/doc/stable/reference/generated/numpy.ndarray.copy.html

[^1]: *Not true for advanced indexing*
