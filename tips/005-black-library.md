# Library recommendation: Black

This (very opinionated) tool will format your Python code and let you move
on with actual programming. It is minimally-configurable, deterministic,
complies with PEP8, and used by some high profile codebases and organziations.
Ever since I've added it to my Python toolbelt, I have never looked back: no
more bickering about code style.

```bash
pip install black                      # Install
python -m black path/to/code/ --check  # Show changes
python -m black path/to/code/          # Apply formatting
```
It really is that simple to make your code style more consistent and readable.

Wonder why I use double-quotes instead of the more idiomatic single-quotes
in Python? Because Black prefers double-quotes. As Henry Ford is often
(mis?)quoted as saying: "You can buy the Model-T in any color you like, as
long as it's black."

Check it out: https://github.com/psf/black
