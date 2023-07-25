# User Data Directories

We just wrote a new app and we have some configuration to save and large files to cache. Linux and other Unix systems follow a convention for locating such user data directories. The home directory can be found at `/home/username` (aka `~`) and is generally where all user-specific data is saved:
```python
from pathlib import Path

home = Path.home()  # "/home/ariel"
```

Within the home directory there are more subdivisions. User-specific configuration (such as settings) can be found at `~/.config/`, and we can create our own files and folders there. This is where we will save the user's configuration files.
```python
import os
assert os.name == 'posix'  # Assuming on Linux
# ~/.config/config.toml
config = home / ".config" / "config.toml"
```

User-specific non-essential data (such as cached downloads) can be found at `~/.local/share/`, and we can create our own files and folders there as well. This is where we will save the user's downloads for later reuse:
```python
import os
assert os.name == 'posix'  # Assuming on Linux
# ~/.local/share/cached_downloads/file.zip
cached_downloads = home / ".local" / "share" / "cached_downloads"
cached_downloads.mkdir(parents=True)
```

References:
https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html
