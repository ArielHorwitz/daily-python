# Hashing and Mapping

Hashing is a very important concept in software and information security. It can sound confusing for the uninitiated, but really it's just a lot of words for a simple idea. The *tl;dr* breakdown:
- A *hash function* takes data and returns a *digest*
- Digests are (usually) fixed-size regardless of input size
- The same data will always result in the *same digest*
- Changing even a single bit in the data should result in a *very different* digest [^1]

To simplify even more: **hashing creates an "ID number" for *any* data** (also known as *mapping*). This ability is extremely powerful and enables things like dictionaries and sets: when a dict/set checks an object for membership, it is only checking the small hash value (digest) of the object's data. This is why a dict/set can only contain keys that are hashable and why two objects that return the same hash value will be considered the same object in sets and dictionary keys.

So an object's hash function should be derived from all data that can make it *distinct*. Python's builtin `hash` function calls an object's `__hash__` special method, which must return an integer. Normally we just add all the relevant values to a tuple and hash that:
```python
class Weather:
    def __init__(self, temp, humidity, debug):
        self.temp = temp
        self.humidity = humidity
        self.debug = debug

    def __eq__(self, other):
        # Required __hash__ to work
        return hash(self) == hash(other)

    def __hash__(self):
        return hash((self.temp, self.humidity))

sunday = Weather(25, 0.5, "sunday morning")
monday = Weather(25, 0.5, "monday noon")
tuesday = Weather(21, 0.2, "tuesday night")
mydict = {sunday: None, monday: None, tuesday: None}
myset = {sunday, monday, tuesday}
assert 2 == len(mydict) == len(myset)
```

Python requires that an object that defines `__hash__` must also define `__eq__`, as it would not make sense that objects can be compared by hash but not value. See documentation for more info.

References:
[https://docs.python.org/3/reference/datamodel.html#object.\_\_hash\_\_](https://docs.python.org/3/reference/datamodel.html#object.__hash__)

[^1]: Not really but good enough for this intro (see: hash collision)
