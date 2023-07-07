# Numpy N-Dimensional Arrays
> This post continues [Introduction to Numpy](/tips/044-numpy-intro.md).

In yesterday's post, we became familiar with numpy. Today we will learn some of the basics for working with numpy. First up is the `ndarray` which in practice is a flat array and a `shape` property. The shape tuple length determines how many dimensions, and the values of the tuple determine how large each dimension is:
```python
import numpy as np

arr = np.array(list(range(8)))  # like: np.arange(8)
assert arr.shape == (8, )
print(arr)
# [0 1 2 3 4 5 6 7]
arr.shape = (2, 4)
print(arr)
# [[0 1 2 3]
#  [4 5 6 7]]
arr.shape = (2, 2, 2)
print(arr)
# [[[0 1]
#   [2 3]]
#
#  [[4 5]
#   [6 7]]]
arr.shape = (2, 2, 2, 1)
# Added another dimension of size 1
arr.shape = (4, 4)
# ValueError: cannot reshape array of size 8 into shape (4,4)
```

Notice how the product of the shapes are all equal to the length of the array. Reshaping an array requires that the new shape exactly fit the amount of elements in the array: `8 = 2 * 4 = 2 * 2 * 2 * 1 != 4 * 4`. I would recommend getting comfortable with the shape property: it is not just how to understand arrays of all dimensions and sizes, but also a large part of how numpy works under the hood (more on this in another post).

Next up is indexing. Since we have multiple dimensions, we can actually take slices (ranges) of each dimension separately, and we separate them using commas. Notice how for each slice we get another dimension in the output:
```python
arr = np.arange(100).reshape(10, 10)
print(arr)
# [[ 0  1 ...  8  9]
#  [10 11 ... 18 19]
#  ...
#  [90 91 ... 98 99]]
print(arr[3, 5])
# 35
print(arr[:2, 2])
# [ 2 12]
print(arr[:2, 7:])
# [[ 7  8  9]
#  [17 18 19]]
```

When we index into an ndarray, numpy will avoid making copies as much as it can, and tries to return an object which points to the same data in memory (the "view"). This is not always the case.

We will continue discussing numpy in posts to come, stay tuned!

References:
https://numpy.org/doc/stable/
