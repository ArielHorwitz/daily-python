# Numpy Advanced Indexing
> This post is part of a series on numpy. See [earlier posts](/).

Advanced (aka "fancy") indexing is how numpy behaves when indexing with an array (or sequence). Many numpy functions return arrays useful for advanced indexing. If the examples feel rudimentary, that is for simplicity of illustration.

Each index array represents the coordinates for that axis as if they were "zipped" together:
```python
import numpy as np

grid = np.arange(20).reshape(5, 4)
# [[ 0  1  2  3]
#  [ 4  5  6  7]
#  [ 8  9 10 11]
#  [12 13 14 15]
#  [16 17 18 19]]

# Get the corners
first_axis = [0, 0, 4, 4]
second_axis = [0, 3, 0, 3]
corners = grid[first_axis, second_axis]
# indexes (0, 0) (0, 3) (4, 0) (4, 3)
assert np.array_equal(corners, [0, 3, 16, 19])
```
Numpy will broadcast these index arrays to a compatible shape if possible. If that is not possible, an `IndexError` is raised. Integers are handled like an array of shape `(1,)` which can always broadcast:
```python
# Get the second element of the first and third row
result = grid[[0, 2], 1]  # == grid[[0, 2], [1, 1]]
assert np.array_equal(inner, [1, 9])

grid[[0, 1, 2], [1, 2]]  # IndexError: shape mismatch
```

Advanced indexing can be mixed with normal indexing, such that we can use slices as well as ellipsis (`...`) to autofill missing dimensions and `np.newaxis` (an alias to `None`) to insert a new dimensions:
```python
# Get the top and bottom rows
result = grid[[0, 4]]  # == grid[[0, 4], ...]
assert np.array_equal(result, [[0, 1, 2, 3], [16, 17, 18, 19]])

# Get second and fourth element from every other row
result = grid[::2, [1, 3]]
assert np.array_equal(result, [[1, 3], [9, 11], [17, 19]])

# Add dimension to the first two rows
result = grid[[0, 1], np.newaxis, ...]
assert result.shape == (2, 1, 4)
assert np.array_equal(result, [[[0, 1, 2, 3]], [[4, 5, 6, 7]]])
```

We can even use multidimensional index arrays, though this becomes quite more complicated (in particular when concerned with subspaces):
```python
# Multidimensional subspaces
second = np.array([[0, 1], [1, 2], [2, 3]])
result = grid[-1, second]
assert result.shape == second.shape
assert np.array_equal(result, [[16, 17], [17, 18], [18, 19]])

first = np.array([0, 2]).reshape(1, 1, 2)
second = np.array([[0, 1], [1, 2], [2, 3]])
assert first.shape == (1, 1, 2)
assert second.shape == (3, 2)
broadcast_shape = (1, 3, 2)
assert grid[first, second].shape == broadcast_shape
```

> *In general, the shape of the resultant array will be the concatenation of the shape of the index array (or the shape that all the index arrays were broadcast to) with the shape of any unused dimensions (those not indexed) in the array being indexed.*
>
> -- Numpy user guide

By following the above rule of thumb, you can understand almost all use cases for advanced indexing. For more details, see the documentation and user guide.


References:
https://numpy.org/doc/stable/user/basics.indexing.html#advanced-indexing
