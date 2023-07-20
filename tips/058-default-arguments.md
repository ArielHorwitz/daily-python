# Handling Default Arguments

Python allows us to make arguments optional by giving them a default value. The default arguments are not in the function body's scope. That means that mutable default arguments are not created for each call:
```python
class Node:
    def __init__(self, neighbors=[]):
        self.neighbors = neighbors

a = Node()
b = Node()
assert a.neighbors == b.neighbors == []
# These lists are literally the same object
assert a.neighbors is b.neighbors
a.neighbors.append(b)
assert b in a.neighbors
assert b in b.neighbors  # Oops!
```

In the above example, Python creates an empty list as a default argument for `Node.__init__()`. This list is essentially living in the parent scope and is reused every time. Hence, each node shares the same list. Assuming that's not what we wanted, it is better to use a "sentinel value" such as `None`.

We can also use the ternary expression `value if (statement) else default`:
```python
class Node:
    def __init__(self, neighbors=None):
        self.neighbors = [] if neighbors is None else neighbors

a = Node()
b = Node()
assert a.neighbors == b.neighbors == []
# These lists are separate
assert a.neighbors is not b.neighbors
a.neighbors.append(b)
assert b in a.neighbors
assert b not in b.neighbors  # Correct!
```

If you must distinguish between `None` and no argument passed at all, you can simply create another sentinel object:
```python
_SENTINAL = object()

def frobnicate(config=_SENTINAL):
    if config is None:
        ...  # None = user defaults
    elif config is _SENTINAL:
        ...  # Nothing =  system defaults
    else:
        ...  # Anything else = custom
```

We make sure to use `is` to be as explicit as possible, however we *can* be less explicit:
```python
class Node:
    def __init__(self, neighbors=None):
        self.neighbors = neighbors or []
```

Why does this work? When Python encounters `a or b` it will evaluate that entire statement by first evaluating `a` and if that is not True, it will evaluate `b`. For this reason the statement `True or func()` will never call `func`.

In Python "truthy"-ness is given by an object's `__bool__` method. The base object in Python is truthy, making all other objects truthy unless they or a parent class overrides this behavior. Consider some of the builtin objects:
```python
negative = "NEGATIVE"
# Not truthy
assert None or negative is negative
assert False or negative is negative
assert [] or negative is negative  # Empty sequences should be False
# Truthy
assert True or negative is not negative
assert -1 or negative is not negative
assert object() or negative is not negative
assert object or negative is not negative
assert type(type) or negative is not negative
```

Noticing this behavior in a `var = arg or default` can be challenging. Python prefers explicit over implicit, and in this case it is wise advice.
