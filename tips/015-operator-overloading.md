## Operator overloading

Operator overloading is a feature that allows you to define custom behavior for built-in operations like arithmetic, comparisons, and boolean operations. By implementing special methods in your class, you can define how instances of your class should behave as operands for built-in operators such as `+`, `-`, `*`, `==`, `<`, etc. We do this by respectively defining and overriding (or *overloading* Python's builtin) methods such as `__add__`, `__sub__`, `__mul__`, `__eq__`, `__lt__`, etc.

Suppose we have an `EventLog` class that is used everywhere, and it's instances are being combined with eachother. We decide it is best if we could use the syntax `log1 + log2`. We also want them to be sorted because it would be very confusing to look at shuffled event logs. Let's see how to implement this operator overloading:
```python
import time

class Event:
    def __init__(self, message):
        self.time = time.time()
        self.message = message

    def __lt__(self, other):
        # Allow two Events to be compared in size
        # using the `<`  operator (for sorting)
        return self.time < other.time

class EventLog:
    def __init__(self, events=None, /):
        self.events = [] if events is None else events

    def append(self, event):
        self.events.append(event)

    def __add__(self, other):
        # Allow two EventLogs to be combined (and sorted)
        # using the `+` operator
        return EventLog(sorted(self.events + other.events))

# Creating two event logs
event_log = EventLog()
net_log = EventLog()

# Adding alternating events
event_log.append(Event("Initializing..."))
net_log.append(Event("Connection established."))
event_log.append(Event("Network initialized."))

# Combined log, automatically sorted by time
combined_log = event_log + net_log

messages = (e.message for e in combined_log.events)
print("\n".join(messages))
```

Note how the built-in `sorted()` function is able to sort any object that implements `__lt__`. This is a very basic example and there is much more to operator overloading such as type checking and duck typing, emulating types, chained comparisons, domain-specific languages, and more.

Python allows for great flexibility and expressiveness with operator overloading. However, take care to ensure that your implementation aligns with Python's principle of readability and explicitness, as well as remaining intuitive or consistent with the normal behavior of built-in operators.

References:
- https://docs.python.org/3/reference/datamodel.html#special-method-names
- https://docs.python.org/3/reference/datamodel.html#emulating-generic-types
