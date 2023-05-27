## The requirements.txt file

Yesterday we discussed virtual environments. Today we'll discuss a simple method to
configure these virtual environments: the `requirements.txt` file. This file is nothing
more than a list of libraries that we can ask pip to install.

As a developer of a project you may decide to provide a requirements file for your
users, which would let them know what dependencies to install to run your code. This
file should be updated every time you add a new dependency to your project. Creating and
updating the requirements file is simple:
```bash
# Save all external package names along with their version
pip freeze > requirements.txt
# Add the updated requirements file to version control
git add requirements.txt && git commit -m "Updated requirements.txt"
```

As a user or developer, you can always update your own environment:
```bash
# Make sure we have the latest version
git pull
# Install all dependencies (will skip up-to-date dependencies)
pip install -r requirements.txt
```

### Controlling dependency versions
Sometimes, it is important to specify a range of versions or particular version for a
library. This may be because you wish to always use the latest version of a dependency,
or to avoid an update to a library that breaks the expected behavior of your own code,
or for security reasons where a specific version is to be avoided.
```
# Example requirements (whitespace is optional)
black == 23.3.0         # Use this exact version
black >= 23.2, < 24     # Use the latest version since 23.2, that is 23.x
black ~= 23.2           # Shorthand for the line above
black >= 22.1, != 22.4  # Use latest version since 22.1, but not 22.4
```

### Dependency chains
When installing a library, pip will automatically also install all of that library's
dependencies (e.g. using that package's `project.toml` file, a topic for another day).
This means that when we invoke `pip freeze` we get all dependencies of dependencies
recursively. This is not strictly required as just mentioning the top-level dependencies
will cause a cascade of installations and end up with a similar result -- and sometimes
you may wish to keep your `requirements.txt` file organized and readable. For this
reason you can use a tool like [pip-chill](https://pypi.org/project/pip-chill/).

References:
- https://peps.python.org/pep-0508/
- https://pip.pypa.io/en/stable/reference/requirement-specifiers/
- https://peps.python.org/pep-0440/#compatible-release
- https://semver.org/
