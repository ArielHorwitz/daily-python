# Numpy Dimension Expanding And Broadcasting
> This post is part of a series on numpy. See [earlier posts](/).

Numpy has an *enormous* amount of functions. Today I'll be sharing some useful functions I found while researching for this series of posts, dealing with dimensions of course. First is expanding dimensions, which I normally do using indexing:
```python
import numpy as np

arr = np.arange(3)
assert np.expand_dims(arr, 0).shape == arr[np.newaxis, ...].shape == (1, 3)
assert np.expand_dims(arr, 1).shape == arr[..., np.newaxis].shape == (3, 1)

arr = np.zeros((4, 2, 6, 9))
assert np.expand_dims(arr, [1, 2, -1]).shape == (4, 1, 1, 2, 6, 9, 1)

# Remove dimensions of size 1
assert np.squeeze(arr).shape == (4, 2, 6, 9)
```

We also have several functions dealing with broadcasting directly:
```python
a = np.zeros((4, 1, 1))
b = np.zeros((1, 6, 9))

assert np.broadcast_to(a, (4, 3, 2)).shape == (4, 3, 2)
assert np.broadcast_arrays(a, b).shape == np.broadcast_shapes(a, b)
```

References:
- https://numpy.org/doc/stable/reference/generated/numpy.broadcast_arrays.html
- https://numpy.org/doc/stable/reference/generated/numpy.expand_dims.html
