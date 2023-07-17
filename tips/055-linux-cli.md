# Using Python As Linux Scripts

Today we'll start a series on creating a simple command line tool in Linux using Python. We will learn about three standard libraries: `subprocess` for running bash commands, `shlex` for lexing bash arguments, and `argparse` for argument parsing. For today we will create a hello script just to start running our tool with arguments:
```python
#! /usr/bin/python
import argparse

def main():
    parser = argparse.ArgumentParser(description="My hello script.")
    parser.add_argument("NAME", help="Your name.")
    args = parser.parse_args()
    print(f"Hello, {args.NAME}.")

if __name__ == "__main__":
    main()
```

The first line (the "hashbang") tells the program loader which program to use to execute this script. It can be replaced with the path to your Python interpreter. This will enable us to run this script from the command line directly without passing it to a Python interpreter. The last two lines simply ensure that the `main` function only runs when executed directly and not when imported.

Let's save this script as `hello.py`. We want it to be executable like any other script or binary, so we will change the permissions of this file to be executable by anyone on the system:
```console
$ chmod +x /path/to/hello.py
```

And finally, we can run it:
```console
$ /path/to/hello.py "Ariel Horwitz"
Hello, Ariel Horwitz.
```

Notice how we used quotation marks. Let's see what happens if we don't:
```console
$ /path/to/hello.py eggs spam
usage: hello.py [-h] NAME
hello.py: error: unrecognized arguments: spam
```

Notice it's telling us we passed an extra argument. Each word is considered it's own argument, so we use quotation marks to denote that it's all one argument. But it's also showing us how to use it and suggesting we use `-h` for help:
```console
$ /path/to/hello.py -h
usage: hello.py [-h] NAME

My hello script.

positional arguments:
  NAME        Your name.

options:
  -h, --help  show this help message and exit
```

Tomorrow we will start performing commands on our system, stay tuned!

References:
- https://docs.python.org/3/howto/argparse.html
- https://docs.python.org/3/library/argparse.html#module-argparse
