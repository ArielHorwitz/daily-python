# Swapping Variables With Tuple Unpacking

As we have seen in many posts prior, we can use "tuple unpacking" to assign variables to elements of a sequence:
```python
mytuple = (1, 2, 3)
mylist = [1, 2, 3]

one, two, three = mytuple
one, two, three = mylist
```

And of course the other way around works to create tuples:
```python
mytuple = one, two, three
assert type(mytuple) is tuple
assert len(mytuple) == 3
```

Something that we didn't mention is that we can use packing and unpacking in the same statement to swap variables quite easily (whereas usually you would need a third intermediate variable):
```python
yes = False
no = True
# Swap yes and no
yes, no = no, yes
assert yes and not no
```
