# Generators

Yesterday, we discussed the iterator protocol in Python. However we also saw that implementing it was somewhat verbose. Generators are a natural extension of iterators, where the `yield` keyword (and `yield from`) is used to more easily define iterators.

Instead of writing a class we can simply use a function and it's *own local state*. By including the yield keyword it becomes a *generator*. When called, these functions return a *generator object* which can be iterated over:
```python
def myrange(stop: int):
    i = 0
    while True:
        yield i
        i += 1

zero_to_four = list(range(5))
for i in myrange(10):
    ...
```

The `yield from` keyword allows a generator to temporarily defer iteration to another iterator:
```python
from pathlib import Path

def all_paths_children(*dirs):
    for directory in dirs:
        yield from Path(directory).iterdir()

list(all_paths_children("music", "pictures"))
# ["music/file1", "music/file2", "pictures/file1", ...]
```

Generators can be defined using a single statement, which can be easily inserted inline as an argument. This enables everyone's favorite syntactical sugar - generator comprehension:
```python
squares_generator = (i ** 2 for i in range(10))
for squared in squares_generator:
    ...

set_comprehension = {file.parent for file in ALL_FILES}
list_comprehension = [
    str(file) for file in DIRECTORY.iterdir()
]
dict_comprehension = {
  key: value for key, value in zip(keys, values)
}
files_only = [
    child.resolve().relative_to(BASE_DIR)
    for child in DIRECTORY.iterdir()
    if child.is_file()
]
```
Generators (and iterators) enable the "lazy" strategy - which is to perform necessary functions *only when called for*, saving on memory and computation. This is the difference between `range` in Python versions 2 and 3: the first would populate a whole list before iteration, while the latter works more like a generator, only calculating a single number per iteration.

Iterators and generators also introduce the concept of a "resumable" function. This is one conceptual step before the *coroutines* used for asynchronous runtime with the `asyncio` module. We will explore this in a future post. Stay tuned!

References:
- https://peps.python.org/pep-0255/
