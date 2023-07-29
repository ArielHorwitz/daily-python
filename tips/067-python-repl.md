# Using The Python REPL As A Calculator

When running a Python script, Python essentially reads each line in the file and interprets it. The REPL (Read-Eval-Print Loop) is a command-line interface to interactively work with the Python interpreter line by line. The loop will evaluate expressions and print the result (or nothing if the result is `None`):
```console
$ python -q
>>> website = "https://ariel.ninja"
>>> website
'https://ariel.ninja'
>>> def func(arg):
...     return arg + 1
...
>>> func(1)
2
```

When I need a calculator on my computer I have several options, but by far my favorite for quick and simple calculations is using Python's REPL:
```console
>>> 420 / 69 + 1e+3 / 2**10
7.063519021739131
>>> import cmath
>>> eulers_id = cmath.exp(cmath.log(-1))
>>> quit()
```

Best of all, you can run a script before interacting with the interpreter. In this example, we are running a script with two lines passed as a string (`from math import * ; import cmath`) to import some useful imports when performing some quick calculations:
```console
$ python -ic "from math import * ; import cmath"
>>> exp(cmath.sin(pi-e/4).real)
1.874719344323452
>>> _  # An underscore is the name for the last value
1.874719344323452
>>> ceil(_)
2
```

References:
https://docs.python.org/3/tutorial/interpreter.html
