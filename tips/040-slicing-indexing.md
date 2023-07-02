# Slicing and Indexing

Indexing into sequences in Python is done with either an integer index or a `slice` object. The slice object represents a range of indices and has 3 attributes to note: `start`, `stop`, `step` and they all default to `None`. When indexing with semicolons (`:`) a slice object is created with the respective arguments:
```python
mylist = list(range(10))
assert mylist[4] == 4
assert mylist[1:3] == mylist[slice(1, 3)]
assert mylist[::-1] == mylist[slice(None, None, -1)]
```

Python allows us to emulate sequence types, specifically indexing with square brackets, such as `object[key]`. There are 3 methods for this: `__getitem__`, `__setitem__`, and `__delitem__`. Let's create a "lazy loading" list that should essentially never raise an `IndexError`:
```python
class LazyList:
    # A list that will lazily create items when indexed.
    # Requires a "get_next" function to generate items.

    def __init__(self, get_next, iterable=None):
        self._get_next = get_next
        self._list = list(iterable) if iterable else []

    def __getitem__(self, key):
        self._produce_until(key)
        return self._list[key]

    def __delitem__(self, key):
        self._list = self._list[:lowest_index(key)]

    def __setitem__(self, key, value):
        del self[key]
        self._produce_until(key)
        self._list[key] = value

    def _produce_until(self, key):
        while len(self._list) <= highest_index(key):
            self._list.append(self._get_next(self._list))

    ...  # Implement len, bool, repr, iter, etc.

def highest_index(key):
    if not isinstance(key, slice):
        return key
    return -1 if key.stop is None else key.stop

def lowest_index(key):
    if not isinstance(key, slice):
        return key
    return 0 if key.start is None else key.start
```

In the example above, a `LazyList` object should always return an element when indexed. All items after an element that has been deleted or modified will also be removed. This allows us to do things not normally possible in a normal list:
```python
def incrementor(trunk_list):
    return (trunk_list[-1] + 1) if trunk_list else 0

lazy = LazyList(get_next=incrementor)
del lazy[69]
assert lazy[5] == 5    # Automatically generated
lazy[2] = 420          # Modified and truncated
assert len(lazy) == 3
assert lazy[5] == 423  # Automatically generated
del lazy[0]            # Remove all items
assert len(lazy) == 0
```

References:
https://docs.python.org/3/reference/datamodel.html#emulating-container-types
