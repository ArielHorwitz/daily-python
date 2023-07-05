# Dataclasses

The `dataclass` decorator is a code generator for classes. It is typically seen as a more comprehensive method of creating data structures instead of `namedtuples`. By simply decorating our class with `@dataclass` and using some declarative syntax we get a comprehensive amount of code written for us:
```python
from dataclasses import dataclass

@dataclass
class ProcessResult:
    command: str
    returncode: int = 0
    stdout: str = ""
    stderr: str = ""
```

The declarative syntax allows us to simply declare attribute names and specify type hints and defaults. This automagically generates the following special methods: `__init__`, `__repr__`, and `__eq__`. Let's try them out:
```python
ls = ProcessResult("ls", stdout="main.py")
print(ls)
# "ProcessResult(command='ls', returncode=0, stdout='main.py', stderr='')"
assert ls == ProcessResult("ls", stdout="main.py") != ProcessResult("ls")
```

Dataclasses have options that we can pass as arguments to the decorator. The `order=True` parameter generates comparison methods that allow sorting. The `frozen=True` parameter makes these objects immutable and hashable, allowing us to use them in sets and dictionary keys:
```python
from dataclasses import dataclass

@dataclass(order=True, frozen=True, slots=True)
class AudioFile:
    path: str
    format_: str
    length_seconds: float

hello = AudioFile("hello.wav", "wav", 4.20)
phillippic = AudioFile("phillippic.mp3", "mp3", 69)
# Sorted without duplicates
files = sorted(set([hello, phillippic]))
# As keys in a dictionary
sources = {
    hello: "http://example.com/hello.wav",
    phillippic: "http://example.com/phillippic.mp3",
}
```

Finally, we can specify options for each field of the dataclass using the `field` constructor:
```python
from dataclasses import dataclass, field
from time import time

def _get_uuid():
    total = 0
    while True:
        yield (total := total + 1)
get_uuid = _get_uuid().__next__

@dataclass(kw_only=True, slots=True, order=True)
class MouseEvent:
    x: float = field(compare=False, kw_only=False)
    y: float = field(compare=False, kw_only=False)
    button: int = field(default=1, compare=False)
    prev: Optional["MouseEvent"] = field(default=None, repr=False, compare=False)
    uuid: int = field(default_factory=get_uuid)
    time_seconds: float = field(default_factory=time, repr=False)
```

References:
https://docs.python.org/3/library/dataclasses.html
