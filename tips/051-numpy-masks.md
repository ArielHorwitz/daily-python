# Numpy Masks
> This post is part of a series on numpy. See [earlier posts](/).

A mask is a boolean array that represents which elements to operate on in another array. Both the array and mask must be the same shape, such that each element in the array has a corresponding boolean value in the mask. Numpy's advanced indexing allows indexing using masks:
```python
import numpy as np
arr = np.arange(4)
mask = [True, False, True, False]
result = arr[mask]
assert np.array_equal(result, [0, 2])
```

Numpy makes it easy to create masks:
```python
gt_one = arr > 1
assert np.array_equal(gt_one, [False, False, True, True])

even_mask = arr % 2 == 0
assert np.array_equal(even_mask, [True, False, True, False])
assert even_mask.sum() == 2
```

Keep in mind that when indexing with boolean arrays (masks) they are essentially converted into index sequences of all the `True` values. Consider how `np.nonzero(array)` returns a tuple of index arrays suited for [advanced indexing](050-numpy-advanced-indexing.md):
```python
grid = np.arange(12).reshape(4, 3)
# [[ 0  1  2]
#  [ 3  4  5]
#  [ 6  7  8]
#  [ 9 10 11]]
even_mask = grid % 2 == 0
# [[ True False  True]
#  [False  True False]
#  [ True False  True]
#  [False  True False]]
assert even_mask.shape == grid.shape
even_indices = even_mask.nonzero()
# (array([0, 0, 1, 2, 2, 3]), array([0, 2, 1, 0, 2, 1]))
assert len(even_indices) == len(even_mask.shape)
flat_evens = grid[even_indices]
assert np.array_equal(flat_evens, [0, 2, 4, 6, 8, 10])
```

Keep this in mind if you use both boolean and integer arrays:
```python
arr = np.array([
    [3, 1, 4, 1, 5],
    [4, 2, 0, 6, 9],
    [0, 1, 1, 3, 4],
    [1, 2, 3, 4, 5],
    [1, 0, 1, 0, 1],
])

row_sums = arr.sum(axis=1)
mask_rows = row_sums > row_sums.mean()
columns = np.array([-3, -2, -1])

incorrect = arr[mask_rows, columns]
# Oops! Arrays [0 1 3] and [-3, -2, -1]
# will index (0, -3) (1, -2) (3, -1)
assert np.array_equal(np.flatnonzero(mask_rows), [0, 1, 3])
assert incorrect.shape == (mask_rows.sum(), )

correct = arr[mask_rows][:, columns]
again_correct = arr[np.ix_(mask_rows, columns)]
assert np.array_equal(correct, again_correct)
assert correct.shape == (mask_rows.sum(), len(columns))
```

Masks are incredibly useful in numpy. The `MaskedArray` in numpy is a testament to their use in protection against missing and invalid data while performing routine operations. See the documentation for more details.

References:
- https://numpy.org/doc/stable/user/basics.indexing.html#advanced-indexing
- https://numpy.org/doc/stable/reference/maskedarray.html
