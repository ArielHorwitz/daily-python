# Dot Notation For Numbers

The dot notation in Python is supported for numbers. This rare syntax involves adding whitespace before the dot:
```python
# Dot notation on string literals
assert "https://ariel.ninja".isascii()
# Dot notation on integers
assert 1 .to_bytes() == b'\x01'
# Dot notation on floats
assert 1.2 .is_integer() is False
```

While this may be surprising and perhaps even useful in specific circumstances, this syntax may be unfamiliar and confusing. Python strongly advocates explicitness over implicitness:
```python
# Prefer this
assert int.to_bytes(1) == int(1).to_bytes() == b'\x01'
assert float.is_integer(1.2) is float(1.2).is_integer() is False
```

This unusual syntax is also supported for complex numbers, however the syntax is so unintuitive that I can't figure out the parsing logic. I am actually unsure of where this is documented (if you do please be in touch).
