# Functools Caching

The functools module offers a very convenient memoizing decorator for pure functions: the `lru_cache`. This is essentially a wrapper for functions that caches arguments and their return values, to make multiple calls for the same results quicker:
```python
from functools import lru_cache

@lru_cache(maxsize=None)
def translate(word, origin_lang, target_lang):
    # Calls an online API for translation
    # Is quite slow unless cached
    ...
```

This is roughly equivalent to:
```python
translation_cache = {}

def translate(word, origin_lang, target_lang):
    args = (word, origin_lang, target_lang)
    if args in translation_cache:
        # We already have the result in our cache,
        # just return that, it's super quick!
        return translation_cache[args]
    # We don't know the answer yet :/
    # Make the slow API call...
    result = call_api(*args)
    # Save the result for next time, it won't change!
    translation_cache[args] = result
    return result

# This will call the API and save the
# result before returning to us
hello_in_russian = translate("hello", "en", "ru")

# This time we just get the last result from the cache
hello_in_russian = translate("hello", "en", "ru")
```

This is obviously extremely useful for improving performance, but we must consider the following:
- On long running processes with many calls this dictionary can get very large, unless we limit the *maxsize*
- This avoids calling the underlying function if the arguments have been seen before, preventing any side-effects
- Arguments to this function must be hashable, since dictionary keys must be hashable

References:
https://docs.python.org/3/library/functools.html#functools.lru_cache
