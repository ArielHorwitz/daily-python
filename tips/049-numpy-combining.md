# Numpy - Combining Multidimensional Arrays
> This post is part of a series on numpy. See [earlier posts](/).

Numpy has two useful functions for combining arrays: `np.concatenate()` for combining along an existing axis (dimension), and `np.stack()` for combining along a new axis. We will focus on the `axis` keyword which is like an index on the shape property, and is found in many numpy functions. It's default is `0`.

Consider combining three color channels of 32x32 pixels into a single array for the full color image:
```python
import numpy as np
red_channel = np.ones((32, 32))
green_channel = np.ones((32, 32))
blue_channel = np.ones((32, 32))
image = np.stack([red_channel, green_channel, blue_channel])
assert image.shape == (3, 32, 32)
```

Now we have an array of shape `(3, 32, 32)`, where the first dimension represents the color channels, and the other two dimensions represent the pixel coordinates. Suppose you wish to arrange it as `(32, 32, 3)` such that each pixel has 3 colors rather than each channel has 32x32 pixels. We can use the ubiquitous `axis` keyword argument:
```python
channels = [red_channel, green_channel, blue_channel]
image = np.stack(channels, axis=2)
assert image.shape == (32, 32, 3)
```

To combine arrays along an existing axis, we can use `np.concatenate()`. All dimensions except for the concatenation axis must match exactly. Hence in the following case we must add another dimension (of size 1) as axis 0:
```python
# Given 10 full color images of 32x32: (10, 32, 32, 3)
all_images = np.ones((10, 32, 32, 3))
# And a new full color image of 32x32: (32, 32, 3)
new_image = np.zeros((32, 32, 3))
# Conform the new_image (add dimension on the left)
# Three ways to add a new axis:
new_images = new_image.reshape(1, 32, 32, 3)
new_images = new_image[np.newaxis, :, :]
new_images = new_image[None, ...]
# Concatenate (axis=0)
all_images = np.concatenate((all_images, new_images))
assert all_images.shape == (11, 32, 32, 3)
```

And finally, suppose we wanted to add a fourth alpha channel. We wish to add it to the last dimension:
```python
channel_shape = [*all_images.shape[:-1], 1]
alpha_channel = np.ones(channel_shape)
assert alpha_channel.shape == (11, 32, 32, 1)
all_images = np.concatenate((all_images, alpha_channel), axis=-1)
assert all_images.shape == (11, 32, 32, 4)
```

References:
- https://numpy.org/doc/stable/reference/generated/numpy.stack.html
- https://numpy.org/doc/stable/reference/generated/numpy.concatenate.html
