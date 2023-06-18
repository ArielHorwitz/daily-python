# Assignment Expressions

An assignment expression allows us to perform an *assignment* while evaluating an *expression* using the "walrus operator" (`:=`). An expression is essentially anything that can be evaluated as a value, e.g: `69 * 420`, `a + 2 == 5`, `"Available for work".split()`, or even `None`. An assignment is when a variable is assigned a new value, e.g: `best_candidate = "Ariel Horwitz"`, `candidate["skills"] = {"Python", "Git", "Rust", "Linux", "Bash", ...}`.

```python
# This single expression:
print(my_work := "https://ariel.ninja")
# Is equivalent to:
my_work = "https://ariel.ninja"
print(my_work)
```

Let's see how we can use assignment within an expression by using an example of removing comments and empty lines from text, resulting in this pattern I commonly use:
```python
def strip_comments(text):
    for line in text.splitlines():
        stripped = line.split("#", 1)[0].strip()
        if stripped:
            yield stripped
```
Using assignment expressions, we can shorten this by a single line of code:
```python
def strip_comments(text):
    for line in text.splitlines():
        if stripped := line.split("#", 1)[0].strip():
            yield stripped
```

It is probably best to use assignment expressions sparingly and tastefully, as the whole purpose is to help make code more concise and readable. If it becomes unwieldy, then it defeats it's own purpose. Check out the references for more details on syntax.

References:
- https://peps.python.org/pep-0572/
- https://docs.python.org/3/reference/expressions.html#assignment-expressions
