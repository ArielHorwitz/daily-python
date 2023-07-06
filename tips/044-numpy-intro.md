# Introduction To NumPy

NumPy (short for "numerical python") is a Python library that offers blazingly-fast vector arithmetic. Python is notoriously slow compared to compiled languages, and NumPy's strongest form is when performing operations on large sets of numbers. The secret to numpy's speed is being written in C to work with more primitive objects directly, and to avoid Python's dynamic and costly objects and loops. If you are new to numpy some of this may seem alien, but trust me it's worth it if you want to do almost anything in math using Python. It is also helpful in understanding many other libraries that depend on numpy such as the well-known pandas.

Let's assume we have a huge list of 3D points, and we wish to normalize them all (have sum of coordinates equal to 1 while maintaining proportions):
```python
from random import random
POINTS = tuple(tuple(random() for i in range(3)) for j in range(1_000_000))
```

In pure Python, we may implement a solution like so:
```python
def normalize(vectors):
    def normalize_single_vector(vector):
        factor = sum(vector)
        return tuple(c / factor for c in vector)
    return list(map(normalize_single_vector, vectors))
```

The keen-eyed will notice that there are 2 loops in this function. But with numpy we might do something like this:
```python
import numpy as np
NP_POINTS = np.array(POINTS)
def np_normalize(vectors):
    return vectors / vectors.sum(axis=1)[:, None]
```

Let's parse the code snippet above. First, we convert the normal list into a numpy array, a.k.a. `ndarray` (*N-dimentional array*). While numpy functions can take lists as parameters, they will be converted anyway so we just do this explicitly. Next, the operations are the same except in numpy we have a notion of multidimensionality. This is what lets us simply perform operations on the entire list of points: "divide the vectors by the sum of vectors along the 1-th axis".

In numpy, `[1, 2, 3] + [3, 2, 1]` is equivalent to `[1 + 3, 2 + 2, 3 + 1]` or `[4, 4, 4]`. This extends much further, allowing us to handle arrays of virtually any "shape". So in this case we have a list of 1 million points with 3 coordinates each resulting in the shape `(1,000,000, 3)`. In numpy it is relatively trivial to operate on arrays with different shapes and sizes, e.g. 10 groups of 300 images of 2 axes of pixels of 3 colors, resulting in the shape `(10, 300, 2, 3)`. We will discuss the notion of "broadcasting" (fitting arrays of different shapes together) in a future post.

Just to confirm correctness (considering floating point rounding errors):
```python
assert np.allclose(normalize(POINTS), np_normalize(NP_POINTS))
```

And what did we get from this, other than avoiding writing a loop for each dimension?
```python
from timeit import timeit

print("Timing comparison...")
normal_time = timeit(lambda: normalize(POINTS), number=3)
np_time = timeit(lambda: np_normalize(NP_POINTS), number=3)
ratio = normal_time / np_time
print(f"NumPy is ~x{round(ratio)} faster ({np_time} / {normal_time} seconds)")
# Roughly 30(!) times faster in my tests. Your milage may vary.
```

Numpy is a large and mature library, providing many many features. Hopefully we can cover many aspects in posts to come. In the meantime, I highly recommend learning from my favorite numpy guru: Alex Chabot-Leclerc. They have several talks on youtube with excellent tutorials. Stay tuned!

Documentation:
https://numpy.org/doc/stable/user/
