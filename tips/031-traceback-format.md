# Formatting Tracebacks

While handling exceptions, I frequently find myself wishing to log the traceback to a file while still catching and handling the exception. This happens often while debugging long-lived processes that handle requests, but should still work even if a single request fails. The `traceback` module has some handy functions for this, but I tend to use just one:
```python
import traceback

try:
    ...
except Exception:
    # Handle the exception
    ...
    # Log the error
    formatted_traceback = traceback.format_exc()
    log_error(formatted_traceback)
    # Or alternatively print normally to console
    traceback.print_exc()
```

References:
https://docs.python.org/3/library/traceback.html
