# JSON For Arbitrary Data Structures

In a [previous post]([url](/029-pickle.md)) we discussed serialization with the pickle module, and its usefulness (particularly for nested data structures with custom classes of objects), and it's security (or lack thereof). In this post we will discuss the builtin `json` module, it's basic usage, and the unfortunate inconvenience of working with custom classes of nested data structure.

The builtin json module is very simple and easy with builtin classes that match the structure of JSON arrays and objects:
```python
import json

data = {
    "array": [
        "https://ariel.ninja",
        3.14,
        {"nested object": 420},
    ],
    "string": "Available for work",
    "number": 69,
}

serialized_data = json.dumps(data)
assert isinstance(serialized_data, str)
deserialized_data = json.loads(serialized_data)
assert data = deserialized_data
```

That being said, trying to serialize objects that are not subclassed from the few builtin types supported by the module will result in a `TypeError`. For such situations, the objects must be converted to the supported builtin types. A common solution is to use the object's `__dict__` attribute to obtain names and values of explicitly defined attributes.

To illustrate the point made in the post about pickle serialization, [here is an example solution](https://gist.github.com/ArielHorwitz/b2989876e42e2852c491f2cec97814f9) that is somewhat generalized to serialize and deserialize objects of arbitrary classes and nesting. It is quite obviously much more involved and requires class-specific implementations.

References:
https://docs.python.org/3/library/json.html
