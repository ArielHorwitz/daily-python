# Library recommendation: docopt-ng

This one is an easy recommendation for argument parsing. I'll let the readme speak for itself because it's just that elegant:
```python
"""Naval Fate.

Usage:
  naval_fate.py ship new <name>...
  naval_fate.py ship <name> move <x> <y> [--speed=<kn>]
  naval_fate.py ship shoot <x> <y>
  naval_fate.py mine (set|remove) <x> <y> [--moored | --drifting]
  naval_fate.py (-h | --help)
  naval_fate.py --version

Options:
  -h --help     Show this screen.
  --version     Show version.
  --speed=<kn>  Speed in knots [default: 10].
  --moored      Moored (anchored) mine.
  --drifting    Drifting mine.

"""
from docopt import docopt

if __name__ == "__main__":
    argv = ["ship", "Guardian", "move", "100", "150", "--speed=15"]
    arguments = docopt(__doc__, argv)
    print(arguments)
```
results in:
```python
{'--drifting': False,
 '--help': False,
 '--moored': False,
 '--speed': '15',
 '--version': False,
 '<name>': ['Guardian'],
 '<x>': '100',
 '<y>': '150',
 'mine': False,
 'move': True,
 'new': False,
 'remove': False,
 'set': False,
 'ship': True,
 'shoot': False}
```

What are you waiting for? Go check it out!

https://github.com/jazzband/docopt-ng
