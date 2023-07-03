# Set Collections

The `set` is a data type that enables fast operations on collections of unordered distinct objects. In Python, the builtin set requires that the objects be *hashable* (more on that later). That they be distinct is critical, since objects in a set are compared by their hash value and not the objects themselves. This also means that duplicates essentially "automatically disappear" from the set.

```python
# Only one of each distinct object can exist in a set
my_skills = {"python", "python", "git", "linux", "rust"}
my_skills.add("linux")
assert len(my_skills) == 4

# Membership testing
assert "linux" in my_skills
assert "cobol" not in my_skills

# May be shown in some other order
print(my_skills)  # {'linux', 'git', 'python', 'rust'}

# Objects must be distinct, 0 and False have the same value
assert {0, False} == {False} == {0}
```

Sets are most useful for collection membership comparisons. This includes checking if a single object exists in the set as seen above, but also to compare all elements of multiple sets. For this reason I always thinks about the results of set operations as *"items that belong in..."* :
```python
inserter = {"Iron plate", "Gears", "Electronic circuit"}
splitter = {"Iron plate", "Electronic circuit", "Transport belt"}

# Items in inserter OR splitter
all_ingredients = inserter | splitter
# Items in inserter AND splitter
shared_ingredients = inserter & splitter
# Items in splitter MINUS items in inserter
splitter_only = splitter - inserter
# Items in inserter MINUS items in splitter
inserter_only = inserter - splitter
# Items in inserter XOR splitter (one of but not both)
unique_ingredients = inserter ^ splitter
# Items in inserter_only OR splitter_only
also_unique = inserterr_only | splitte_only
assert unique_ingredients == also_unique
```

The immutable version of `set` is `frozenset`. Sets are closely related to *hashing* and *mapping*, which in turn will help us understand dictionaries. Stay tuned!

References:
https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset
