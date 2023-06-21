# Serialization With Pickle

> **DANGER!** Pickle is not considered safe to use, unless you are _confident_ you can trust data you are unpickling.

Python has a very powerful, some might say **_magical_**, serialization module named `pickle`. This module allows you to convert virtually any Python object to a serialized string or bytes, which can then be deserialized by another instance of Python.

For example, if I have a data type that is very complex it may be very difficult or expensive to implement converting it to bytes and back in order to send across a network or save to disk for later use.
```python
class GameState:
    # Very complex data class:
    # nested, full of custom data types,
    # recursive, and circularly referenced
    ...

def save_game(state, filename):
    with open(filename, "wb") as f:
        pickle.dump(state, f)

def load_game(filename):
    with open(filename, "rb") as f:
        return pickle.load(f)

# Save game, close the app, reopen, load game
```

If you've tried other methods for serialization then this feels so easy, it's just absolutely magical. In fact, the loader doesn't even need to define the class it is loading or have the same version, it will just work!

And herein lies the catch: you can think of pickle as saving the actual Python code of the object, along with the values of it's attributes. Which means if you load a pickled object and it has come from a malicious source, they can essentially execute any arbitrary code(!) :
```python
import os
class EvilObject:
    def __reduce__(self):
        return os.system, ("evil-command", )
```

It seems difficult or impossible to inspect a pickled object without unpickling it first. Which is why you _must trust the source_. Solutions to this may be to use and verify cryptographic signatures of the data being unpickled, or to simply use another serialization method, such as JSON which we will discuss in another post.

References:
- https://docs.python.org/3/library/pickle.html
- https://medium.com/ochrona/python-pickle-is-notoriously-insecure-d6651f1974c9
