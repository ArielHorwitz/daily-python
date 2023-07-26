# For Else

Python loops have an `else` clause, allowing you to check if a loop has terminated "normally" (as in it succeded reaching the last iteration). Or, to put it another way, think of it as a `nobreak` keyword (a block of code that executes if there was ***no break***). In a for loop, it may look something like this:
```python
def get_higher(iterable, target):
    for element in iterable:
        if element > target:
            break
    else:
        raise ValueError("no higher number in iterable")
    return element
```

Note that by switching `break` with `return` we can get rid of the `else`. While loops can also use the `else` clause:
```python
condition = True
while condition:
    ...
else:
    assert not condition
```

In the majority of situations, you do not need to use `else` with loops in Python, as the same can be achieved without it.

References:
https://docs.python.org/3/tutorial/controlflow.html
