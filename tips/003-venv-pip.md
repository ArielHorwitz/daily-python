## Python Virtual Environments

A virtual environment is a "copy" of Python that is meant for a single project only.
This is always the recommended way to locally manage packages (external libraries) in
Python.

Virtual environments allow you to make sure each project lives in the environment
specifically designed for it and limits the installed packages that Python is able to
import. It's main function is to allow for reproducable environments across distribution
targets. This can avoid problems that arise from inconsistent behavior of different
package versions or conflicting names from packages in unrelated projects. It is a
similar idea to that of containerization (distribution of Python packages will be
discussed in a future post).
```bash
[ariel@ninja ~]$ mkdir hireme && cd hireme  # Create project folder
[ariel@ninja hireme]$ python -m venv venv  # Create virtual environment
[ariel@ninja hireme]$ printf "\n/venv\n" >> .gitignore  # Ignore in version control
[ariel@ninja hireme]$ source ./venv/bin/activate  # Activate the virtual environment
(venv) [ariel@ninja hireme]$
# Our shell is now using our new virtual environment.
# Libraries we install will be installed for this venv only.
```

### Installing Packages using `pip`
You can install external Python packages from a variety of sources:
```bash
# Install BotRoyale from PyPI, the Python Package Index
pip install botroyale
# Install BotRoyale from the repository on GitHub
pip install git+https://github.com/ArielHorwitz/botroyale.git
# Install a specific branch name or commit hash
pip install git+https://github.com/ArielHorwitz/botroyale.git+branch-or-commit
# Install BotRoyale from a local directory
git clone https://github.com/ArielHorwitz/botroyale.git
pip install ./botroyale
# Install BotRoyale in "develop mode"
pip install --editable ./botroyale
```
In the last example, we install from a local directory as editable. This allows you to
make changes to the library locally without needing to reinstall for effect.


### Alternative tools
- Poetry - https://python-poetry.org/docs/
- Pipenv - https://pypi.org/project/pipenv/


References:
- https://docs.python.org/3/library/venv.html
- https://peps.python.org/pep-0405/
- https://pip.pypa.io/en/stable/cli/pip_install/
