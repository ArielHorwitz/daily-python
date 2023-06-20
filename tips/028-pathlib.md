# Pathlib and the Path Object

A series of Python tips would be remiss without mentioning `pathlib.Path`, one of my favorite builtin modules. The `Path` object is intuitive and saves many headaches when dealing with files and folders and their cross-platform paths.

```python
from pathlib import Path

# Directories
cwd = Path(".")
absolute = cwd.resolve()
my_dir = absolute / "folder" / "path"
assert my_dir.is_relative_to(absolute)
rel_dir = my_dir.relative_to(absolute)
my_dir.mkdir(parents=True, exist_ok=True)

# Directory contents
files = [f for f in cwd.iterdir() if f.is_file()]
dirs = [d for d in cwd.iterdir() if d.is_dir()]
recursive_py = sorted(my_dir.glob("**/*.py"))

# Files
myfile = my_dir / "myfile.ext"
assert myfile.stem == "myfile"
assert myfile.suffix == ".ext"
myfile.write_text("Available for hire.\n")
assert myfile.exists()
myfile.stat()
text = myfile.read_text()
myfile.unlink()
assert not myfile.exists()
```

One of the many thing I like about the Path object is operator overloading that allows "dividing" path parts together, which makes it look like you are actually just writing the literal posix path. Actually, only today I learned of the methods `Path.write_text`, `Path.read_text`, and the corresponding bytes methods. This may be my new favorite feature of the pathlib module.

There is so much to this module and it's virtually all accessible with the one `Path` object. Check out the documentation!

References:
- https://docs.python.org/3/library/pathlib.html
- https://peps.python.org/pep-0428/
