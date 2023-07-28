# Dictionary Default Values

In a [previous post](/tips/018-collections-defaultdict.md) we discussed the `defaultdict` which allows us to create a dictionary that will automatically create key-value pairs and prevent key errors. However the regular dictionary has a few methods that provide similar functionality. The `dict.get()` method will check if the key exists, otherwise returning a default value:
```python
 candidate = dict(
    name="Ariel Horwitz",
    website="https://ariel.ninja",
    available=True,
)

assert candidate.get("available") is True
assert candidate.get("windows_version") is None
assert candidate.get("foo", "default") == "default"
```

The `dict.pop()` method works similarly:
```python
assert candidate.pop("available") is True
assert candidate.pop("bar", -1) == -1
```

And finally we have `dict.setdefault` which will let us set a default value if it's missing and get it back, essentially mimicing the defaultdict behavior on a per-lookup basis:
```python
assert "skills" not in candidate
candidate.setdefault("skills", ["Python"]).extend(["Linux", "Rust"])
assert candidate["skills"] == ["Python", "Linux", "Rust"]
```

References:
https://docs.python.org/3/library/stdtypes.html#dict
