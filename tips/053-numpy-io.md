# Numpy I/O And Compression
> This post is part of a series on numpy. See [earlier posts](/).

Saving and loading numpy arrays to and from binary files is easy. It uses a `.npy` format for a single array:
```python
import numpy as np
rng = np.random.default_rng()
images = rng.random((3, 32, 32))

np.save("images", images)
loaded = np.load("images.npy")

assert type(loaded) is np.ndarray
assert np.array_equal(images, loaded)
```

If you have multiple arrays, you can save and load them all at once using a `.npz` format:
```python
labels = [420, 69, 299_792_458]
exposure = [1/60, 1/30, 5]

# Use `np.savez_compressed` for non-random data
np.savez("labeled", images=images, labels=labels, exposure=exposure)
data = np.load("labeled.npz")

assert type(data) is np.lib.npyio.NpzFile
assert np.array_equal(data["labels"], labels)
assert np.array_equal(data.get("images"), images)
for name, arr in data.items():
    ...
    # images (3, 32, 32)
    # labels (3, )
    # exposure (3, )
```

We can also save and load simple (human-readable) text files such as CSV:
```python
data = [[1, 2, 3], [2, 3, 5], [3, 5, 7]]
np.savetxt('fib.csv', data, delimiter=", ", fmt="%.2f", header="fib.csv")
fib = np.loadtxt("fib.csv", delimiter=",")
# Use np.genfromtxt() for files with missing values
assert np.array_equal(data, fib)
"""
# fib.csv
1.00, 2.00, 3.00
2.00, 3.00, 5.00
3.00, 5.00, 7.00
"""
```

References:
- https://numpy.org/doc/stable/user/how-to-io.html
- https://numpy.org/doc/stable/reference/generated/numpy.save.html
- https://numpy.org/doc/stable/reference/generated/numpy.savetxt.html
