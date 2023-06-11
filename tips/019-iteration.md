# The Iteration Protocol

The concept of iterators in Python is extremely important, and is the basis for a significant amount of the underlying behavior of builtin objects in Python. An iterator is an object that implements the two methods `__iter__()` and `__next__()` which enable using Python's iteration protocol:
```python
class range:
    """Basic iterator like builtin `range()`."""
    def __init__(self, stop: int):
        """Remember iteration parameters."""
        self._index = 0
        self._max_index = stop

    def __iter__(self):
        """Used by container types."""
        return self

    def __next__(self):
        """Generate the next value in sequence."""
        if self._index >= self._max_index:
            raise StopIteration
        self._index += 1
        return self._index - 1

# Done! Let's use our new iterator:
range_ten = range(10)
zero = next(range_ten)
one = next(range_ten)
list_ten = list(range(10))
for i in range(10):
    print(i)
```

You may be able to see Python's iteration protocol at work here. In essense, a for loop behaves like so:
```python
# Python's for loop:
for i in range(10):
    ...

# Is essentially a while loop:
iterator = range(10)
while True:
    try:
        i = next(iterator)
    except StopIteration:
        break
    ...
```

Python's powerful and ergonomic iteration syntax enables elegant code:
```python
for element in reversed(elements): ...
for squared_number in map(lambda n: n ** 2, numbers): ...
for index, element in enumerate(elements): ...
for a, b in zip(elements_a, elements_b): ...
for element, (key, value) in enumerate(zip(elements, mydict.items())): ...
for key in mydict: ...
for value in mydict.values(): ...
for key, value in mydict.items(): ...
```

Classes whose instances are solely responsible for a single iteration loop, which all follow the iteration protocol, are a fundamental concept in Python and the behavior of many builtin objects. This is the basis for generators, another very important concept which also enables memory-efficient sequence generation, as well as for the asyncio loop and list comprehension. We will explore these concepts in future posts. Stay tuned!

References:
https://docs.python.org/3/library/stdtypes.html#iterator-types
