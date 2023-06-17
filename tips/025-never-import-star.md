# Never Import With *

There is a common recommendation to avoid importing with an asterisk:
```python
# lib.py
import math
class CONSTANTS:
    pi = math.pi
    c = 299_792_458
```
```python
# main.py
from lib import *
print(CONSTANTS.c)  # No problem
print(math.e)       # Oops! We imported math from lib
```
Now imagine importing something actually important without realizing, only to create interference and confusion. Using the asterisk import essentially defines names that are not visible anywhere, creating further confusion.

If this is unavoidable and you must use it, or otherwise wish to remove this footgun:
```python
# lib.py
import math
class CONSTANTS:
    pi = math.pi
    c = 299_792_458

__all__ = ["CONSTANTS"]  # List of names imported with *
```
```python
# main.py
from lib import *
print(math.e)  # NameError: name 'math' is not defined
```
