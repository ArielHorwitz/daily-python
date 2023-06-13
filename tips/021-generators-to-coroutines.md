# From Generators To Coroutines
> *It is highly recommended to read the last two posts: [The Iteration Protocol](/tips/019-iteration.md) and [Generators](/tips/020-generators.md).*

There are three main types of concurrency possible in Python:
- **Asynchronous** - "cooperative concurrency" where the *coroutines* (asynchronous functions) yield to each other, allowing one task to run while another is simply waiting for I/O. Async does not improve performance for CPU-bound tasks since no code actually runs in parallel.
- **Multithreading** - due to Python's Global Interpreter Lock, multiple threads cannot run in parallel. This effectively makes multithreading in Python simply another method of getting the benefits of async.
- **Multiprocessing** - true parallelism with multiple parts of code running simultaneously on different processors. Generally recommended only as a last resort.

Today, we will take what we have learned from the last two posts about iteration and generators, and use it to implement an asynchronous framework. To achieve this, we will use generators as coroutines. We will need to keep track of non-complete coroutines, and run a loop that will continuously iterate over them until all tasks are complete:
```python
from time import perf_counter,
from time import sleep as blocking_sleep

# Asynchronous API
TASKS = []
LOOP_INTERVAL = 0.001

def create_task(coro):
    # Add a coroutine to our task list
    TASKS.append(coro)

def run_until_complete():
    # Event loop
    while TASKS:
        select()
        blocking_sleep(LOOP_INTERVAL)

def select():
    # Shuffle tasks
    TASKS.append(TASKS.pop(0))
    # Poll next task and remove if done
    try:
        next(TASKS[0])
    except StopIteration:
        TASKS.pop(0)

def non_blocking_sleep(seconds: float):
    end = perf_counter() + seconds
    while perf_counter() <= end:
        yield

# User code
def mytask(name: str, time: float = 1):
    print(f"Starting task {name!r}")
    # Perform an asynchronous task
    yield from non_blocking_sleep(time)
    print(f"Finished task {name!r}")

start = perf_counter()
create_task(mytask("first", 1.5))
create_task(mytask("second", 1))
create_task(mytask("third", 2.5))
run_until_complete()
elapsed = perf_counter() - start
print(f"Event loop elapsed in {elapsed:.2f}s")
```

This is an extremely simplified example for educational purposes. Real software should use Python's builtin `asyncio` module which uses builtin coroutines using the `async/await` keywords and can wrap them in `Futures` to be managed by the event loop. In a future post we will introduce Python's `asyncio` module and how to use it. Stay tuned!
