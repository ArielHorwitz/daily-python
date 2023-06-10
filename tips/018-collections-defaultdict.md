# The collections defaultdict

Python's builtin `collections` module has some very useful container datatypes. We had already mentioned `namedtuples` and today we will introduce the `defaultdict`, arguably one of the more convenient types of the module. Many Python developers eventually run into this inconvenience:
```python
event_subscribers: dict[set] = {}
def subscribe_to_event(event, subscriber):
    # If the event does not exist in the dict,
    # we must first add it with an empty set
    if event.name not in event_subscribers:
        event_subscribers[event.name] = set()
    # Now it's safe to access the subscriber set
    event_subscribers[event.name].add(subscriber)
```

The `defaultdict` class will create a key-value pair from a given default value factory function (`set` in this case) if a key does not exist:
```python
from collections import defaultdict
event_subscribers: defaultdict[set] = defaultdict(set)
def subscribe_to_event(event, subscriber):
    # It's safe to access the subscriber set
    # because if it does not exist it will be
    # created for us
    event_subscribers[event.name].add(subscriber)
```

References:
- https://docs.python.org/3/library/collections.html
