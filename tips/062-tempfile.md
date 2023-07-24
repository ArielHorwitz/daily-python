# Temporary Files And Folders

Operating systems generally offer a special location for temporary files and folders. Python's builtin tempfile module offers a platform-independent way to find and use these:
```python
import tempfile
from pathlib import Path

# Platform-independent temporary folder and file
_, file = tempfile.mkstemp(suffix=".log")
file = Path(file)                     # e.g. /tmp/tmp1wyp7b2n.log
folder = Path(tempfile.gettempdir())  # e.g. /tmp
assert file.parent == folder

file.write_text("Hire me!")
file.unlink()  # Delete when done
```

By creating the file manually, we are also responsible for deleting it manually (hence `file.unlink()`). If we are interested in keeping this file around during runtime only, we can use the context manager:
```python
with tempfile.TemporaryFile() as fd:
    fd.write(b"https://ariel.ninja")
    fd.seek(0)
    print(fd.read().decode())
```

The tempfile module also has corresponding functionality for temporary directories. Check out the documentation for details.

References:
https://docs.python.org/3/library/tempfile.html
