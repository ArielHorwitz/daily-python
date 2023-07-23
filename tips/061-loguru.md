# Library Recommendation: Loguru

I have always felt that Python's builtin logging module is somewhat dated. My favorite replacement is [loguru](https://github.com/Delgan/loguru), which makes logging simple.

```python
from loguru import logger

logger.debug("That's it, beautiful and simple logging!")
logger.warning("Careful, something seems wrong.")
```

By default, loguru prints colorized and detailed messages to stderr (usually in the terminal). However we can easily configure these to go to file as well:
```python
logger.add("errors.log", level="WARNING", rotation="1 month")
logger.add("trace.log", level="TRACE", rotation="200 MB")
logger.add("debug.log", level="DEBUG", rotation="500 MB", compression="zip")

logger.trace("trace")        # stderr, trace.log
logger.info("info")          # stderr, trace.log, debug.log
logger.critical("critical")  # stderr, trace.log, debug.log, errors.log
```

While I find loguru's defaults to be very good, it offers many features including filters, module-specific levels, formatting, and much more. Check out the documentation.

References:
- https://github.com/Delgan/loguru
- https://loguru.readthedocs.io/en/stable/index.html
