## Packing and Unpacking Sequences and Mappings

Python has ergonomic syntax for packing and unpacking sequences and mappings. Simply add `*` (asterisk) before a sequence (such as a tuple or list) or `**` (double asterisk) before a mapping (such as a dictionary).
```python
first_name, *top_skills = "Ariel python git rust bash".split()
*details, website = "portfolio resume contact https://ariel.ninja".split()
letter, *numbers, number = ["a", *range(5)]
print("Here are the numbers between 0 and 4:", *range(5))

somedict = {0: "zero", 1: "one", 2: "two"}
mydict = {2: "??", **somedict}  # mydict[2] == "two"
mydict = {**somedict, 2: "??"}  # mydict[2] == "??"

def myfunc(a, *all_other_positional, b=..., **all_other_kwargs):
    ...
```

An example of overwriting default keyword arguments by a subclass method or adapter function:
```python
class LocalClient(Client):
    def connect(self, *args, **kwargs):
        kwargs = {**kwargs, "host": "localhost"}  # Override arguments
        return super().connect(*args, **kwargs)
```

References:
- https://peps.python.org/pep-3132/
- https://peps.python.org/pep-0448/
