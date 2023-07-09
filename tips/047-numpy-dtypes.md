# Numpy Data Types
> This post continues our series on Numpy, see [earlier posts](/).

So far while discussing ndarrays, we haven't mentioned the `dtype`. The data type of an ndarray is an object that contains information about the array's data in memory. When creating arrays, numpy automatically detects the dtype and so is normally not very pertinent:
```python
import numpy as np

arr_bool = np.array([True, False])
arr_int = np.array([1, 0])
arr_float = np.array([1., 0.])
assert arr_bool.dtype == np.bool_
assert arr_int.dtype == np.dtype("int")
assert arr_float.dtype == np.dtype(np.float64)
```

### Casting
Ndarrays are *homogeneous* - meaning every element in the array is of the same type. We can specify a particular dtype when creating an array. There are several Python types that numpy will recognize and convert accordingly (`bool`, `int`, `float`, `complex`, `bytes`, `str`). When elements are added or modified in an array, numpy will *cast* them to the correct dtype:
```python
# Numpy will do its best to cast for us, even if data is lost
arr_int = np.array([6, 9, 4, 2, 0.12345], dtype=int)
assert arr_int[4] == 0
arr_float = np.array([6, 9, 4, 2, 0], dtype=float)
arr_float[4] = 0.12345
assert arr_float[4] != 0

# Cast entire arrays:
arr_float = arr_int.astype(float)
assert np.array_equal(arr_int, arr_float)
arr_bool = arr_int.astype(bool)
assert np.array_equal(arr_bool, [1, 1, 1, 1, 0])
```

We can check at runtime what dtypes can be safely *cast* between each other:
```python
assert np.can_cast(int, float)
assert not np.can_cast(int, bool)          # Data will be lost
assert not np.can_cast(np.int16, np.int8)  # Data will be lost
assert np.can_cast(np.int16, np.int8, casting="same_kind")
```

### Overflowing
If you are concerned about overflowing and machine limitations, we can see information about integer and floating point types:
```python
print(np.iinfo(np.int32))           # Display machine parameters for int32
print(np.finfo(np.dtype("float")))  # Display machine parameters for default float

arr_int8 = np.array([1, 0], dtype=np.int8)
assert 325 > np.iinfo(arr_int8.dtype).max
arr_int8[1] = 325  # Overflow! This will fail in future versions of numpy
assert arr_int8[1] == 69
```

We discussed only boolean and numerical types however there are more such as strings, datetimes, and timedeltas. Take a look at the docs for more information. More numpy posts to come, stay tuned!


References:
- https://numpy.org/doc/stable/reference/arrays.dtypes.html
- https://numpy.org/doc/stable/reference/generated/numpy.ndarray.astype.html
