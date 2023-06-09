# Lambda Functions

Lambda functions are small, "anonymous" functions. The intended purpose is for quickly defining a function that needs to be used once, and simply returns the result of a single statement:
```python
nums = list(range(420))
# Sort with named function:
def hash_sorting_key(item):
    return hash(item)
hash_sorted = sorted(nums, key=hash_sorting_key)
# Sort with anonymous function:
hash_sorted = sorted(nums, key=lambda n: hash(n))
```

Lambda functions are called anonymous because they are meant for functions that need to be defined but not named. By saving a lambda function to a variable (naming it) or storing it in a data class, we are kind of extending it's use outside of the intended purpose.

When saving or storing a lambda function, normal scopes of namespaces apply. At first glance you might this this should print the numbers `0..9`, but you'd be wrong:
```python
lambdas = []
for i in range(10):
    lambdas.append(lambda: print(i))
for l in lambdas:
    l()  # prints 9, 9, 9, ...
```
Why is this happening? If you know about namespaces and scopes, here's the answer:
```python
def shared__locals__():
    lambdas = []
    for i in range(10):
        # my_lambda = lambda: print(i)
        def my_lambda():
            print(i)
        lambdas.append(my_lambda)
    for l in lambdas:
        l()  # Prints the same `i`

def exclusive__locals__():
    lambdas = []
    for i in range(10):
        # my_lambda = lambda ii=i: print(ii)
        def my_lambda(ii=i):
            print(ii)
        lambdas.append(my_lambda)
    for l in lambdas:
        l()  # Prints a different `ii` each time
```
