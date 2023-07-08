# Numpy Broadcasting
> This post is part of a series on Numpy - see [previous posts](/).

Numpy arrays behave similarly to lists with some notable exceptions: they have a mutable and multidimensional `shape` (as discussed in our [last post](/tips/045-numpy-ndarrays.md)), and their operations happen *element-wise*. To understand how element-wise operations work in numpy, conisder that arrays can be operands with single values *and* with other arrays:
```python
import numpy as np
arr = np.array([6, 9])

# [6 9] - 2 = [4 7]
assert np.array_equal(arr - 2, [4, 7])

# [6 9] - [2 7] = [4 2]
assert np.array_equal(arr - [2, 7], [4, 2])
```

When operating on arrays, numpy uses a concept called "broadcasting". Before any operation involving arrays, numpy looks at both shapes and tries to fit them together like so:
- Prepend dimensions of size 1 until the length of both shapes ar equal
- Elements in dimensions of size 1 are repeated until the sizes are equal

If this results in equal shapes, the arrays are compatible and can be used as operands:
```python
import numpy as np
from contextlib import contextmanager

def from_shape(*shape): # Return an array full of 1's from given shape
    return np.ones(shape)

@contextmanager
def assert_value_error():
    try: yield
    except ValueError: return
    else: raise RuntimeError("Expected value error")

arr = from_shape(1, 32) + from_shape(32)
assert arr.shape == (1, 32)

arr = from_shape(2, 32) + from_shape(32)
assert arr.shape == (2, 32)

with assert_value_error():
    from_shape(32, 2) + from_shape(32)

arr = from_shape(4, 1, 8) + from_shape(4, 8)
assert arr.shape == (4, 4, 8)

with assert_value_error():
    from_shape(4, 1, 8) + from_shape(4, 2, 4)

# Consider the difference between arrays of these shapes:
# (1, 2) + (2, 2)
# (2, 1) + (2, 2)

# [6 9] + [[1, 2], [3, 4]] = [[ 7 11] [ 9 13]]
assert np.array_equal(arr + [[1, 2], [3, 4]], [[7, 11], [9, 13]])

# [[6] [9]] + [[1, 2], [3, 4]] = [[ 7 8] [12 13]]
assert np.array_equal(arr.reshape((2, 1)) + [[1, 2], [3, 4]], [[7, 8], [12, 13]])
```

For me it took some time to intuit on these shapes, but once I did, understanding operations on arrays of any shape became much easier. I highly recommend you play around with this yourself. We will continue discussing numpy in posts to come, stay tuned!

References:
https://numpy.org/doc/stable/
