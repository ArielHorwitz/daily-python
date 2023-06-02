## F-strings

By now, most Python developers will hopefully be familiar with F-strings (formatted strings). As opposed to %-formatting or str.format(), this is another alternative that can make your string literals more readable:
```python
name = "Ariel Horwitz"
# Python 3.5
print("My name is %s".format(name))
print("My name is %s" % name)
# Python 3.6+
print(f"My name is {name}")
```

Let's take a look at some of the features available in Python string formatting:
```python
mystr = "Available for work"
myint = 42
myfloat = 3.1415926535
print(f"{mystr} for more than {myint} hours a week")  # Normal string formatting
print(f"{myint * 10}")   # Expressions are supported
print(f"{myfloat = }")   # Prints "myfloat = 3.1415926535" (for debugging)
print(f"{mystr:>30}")    # Adjust 30 characters to the right
print(f"{mystr:_^30}")   # Justify within 30 characters and fill with "_"
print(f"{myfloat:.3f}")  # Show as float, rounded to 3 decimal places (3.142)
print(f"{myfloat:.1%}")  # Show as a percentage, rounded to 1 decimal place (314.2%)
print(f"{myint:x}")      # Show integer as hex (2a)
print(f"{myint:b}")      # Show integer as binary (101010)
```

References:
https://peps.python.org/pep-0498/
