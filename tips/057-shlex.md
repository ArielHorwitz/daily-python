# Using Python As Linux Scripts - Part 3
> It is highly recommended to read [part 1](/tips/055-linux-cli.md) and [part2](/tips/056-linux-cli2.md).

Yesterday, created a Python script that we can run like any command line utility. It has argument parsing with help (`volume.py -h`) and performs the tasks we want it to. However when dealing with user-given arguments that are interpereted for execution, it is extremely important to consider not only malformed input but also injection attacks.

For this reason we will have a look at the `shlex` module, for lexing input for shells. And there are really only two functions of interest. The first is for convenience, to split a string to arguments like a terminal would:
```python
import shlex

split = shlex.split("cp -f /dir1/file /dir2/")
assert split == ["cp", "-f", "/dir1/file", "/dir2/"]

split_space = shlex.split("cp '/dir with space/file' /dir2/")
assert split_space == ["cp", "/dir with space/file", "/dir2/"]
```

As we can see, it splits along spaces but keeps words between quotes as a single argument. This helps pass arguments with spaces in them.

The function `subprocess.run()` makes it very difficult to do something unintentional, unless you use `shell=True`. Without going into detail on using `subprocess.Popen`, let's consider when things go wrong to understand why `shell=True` requires extra care:
```python
#! /bin/python
# mygrep.py - don't forget to `chmod +x mygrep.py`
import shlex
import subprocess
filepath = input("enter file path: ")
subprocess.run(f"grep -E SEARCHTERM {filepath}", shell=True)
```

And if we pass `/dir withspace/file` as input:
```console
$ ./mygrep.py
enter file path: /dir withspace/file
grep: /dir: No such file or directory
grep: withspace/file: No such file or directory
```

Oops! The user passes an unquoted filepath with spaces and it was interpereted as multiple arguments. This can be even more malicious if we pass `/ ; printf "\n\nI can run commands!\n"` as input:
```console
$ ./mygrep.py
enter file path: / ; printf "\n\nI can run commands!\n"
grep: /: Is a directory

I can run commands!
```

Yikes! So how do we solve this? Either don't use `shell=True` with user-provided input or make sure to quote it:
```python
argument = shlex.quote("/dir withspace/file")
assert argument == "'/dir withspace/file'"  # has quotes
argument = shlex.quote("/dir2/")
assert argument == "/dir2/"  # no need for quotes
```

If we change `mygrep.py` to use:
```python
...
subprocess.run(f"grep -E SEARCHTERM {shlex.quote(filepath)}", shell=True)
```

then it solves both issues we had earlier:
```console
$ ./mygrep.py
enter file path: / ; printf "\n\nI can run commands!\n"
grep: / ; printf "\n\nI can run commands!\n": No such file or directory
```


References:
https://docs.python.org/3/library/shlex.html
