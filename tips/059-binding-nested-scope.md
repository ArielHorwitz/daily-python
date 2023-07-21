# Name Bindings In Nested Scopes

Python code is divided into scopes. A scope is a block of code that shares a namespace where names of variables and functions are defined.

Scopes can be nested within each other. For example, a module (Python file) is a scope which itself can contain variables and functions bound to names in the module's namespace. The function body is also a (nested) scope which itself can contain variables and functions bound to names in the function's namespace.

Nested scopes can "inherit" names of outer scopes. This means that if a name is not found in the current namespace, it will be searched for in the outer scope and then it's outer scope etc:
```python
def outer_scope():
    name = "Name"

    def inner_scope():
        print(name)  # Prints "Name"

    inner_scope()

outer_scope()
```

However a lesser known fact is that when Python sees a name in scope it will associate it with the scope throughout.
```python
def alice_scope():
    eggs = "spam"
    name = "Alice"

    def bob_scope():
        print(eggs)  # Prints "spam"
        print(name)  # UnboundLocalError
        name = "Bob"

    bob_scope()

alice_scope()
```

In the above example we can see that the variable `eggs` is found from the outer scope. But the variable `name` is bound is both scopes. When we try to print it, one may expect Python to print "Alice" since it is defined in the outer scope with that value. But since we also bind `name` in the local scope (even if it is defined later in the code block), Python will not attempt to find it in an outer scope, and raise an error since we have not associated it with a value when we try to access it.

References:
https://peps.python.org/pep-0227/
