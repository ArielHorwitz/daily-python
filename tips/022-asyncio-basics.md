# Asyncio Basics
> *It is highly recommended to read the last few posts: [The Iteration Protocol](/tips/019-iteration.md), [Generators](/tips/020-generators.md), and [From Generators To Coroutines](/tips/021-generators-to-coroutines.md).*

Yesterday, we looked at how to create an event loop and coroutines using generators. This was simply an interesting thought experiment to demonstrate how an asynchronous runtime could work. However since generators and coroutines actually perform different functions, and the implementation wishes to be agnostic to frameworks, Python has syntax specifically for coroutines: `async` is used to define coroutines, and `await` is used to call them. When "awaiting" a coroutine you are also yielding to the event loop and allowing other coroutines a chance to run, and are expected to yield back.

Calling coroutines from normal functions doesn't make much sense, since a normal function *will not yield* to the event loop - it will not "await" and cooperatively work with the event loop. You may as well just call a normal function. This means that at some point you must start the event loop for any of this to work, which is normally done with `asyncio.run`. And finally, running multiple coroutines concurrently is normally done with `asyncio.gather`.
```python
import asyncio
from time import perf_counter
import requests  # Synchronous
import httpx     # Asynchronous

REQUEST_URLS = (
    "https://en.wikipedia.org/api/rest_v1/page/summary/galaxy",
    "https://en.wikipedia.org/api/rest_v1/page/summary/computer",
    "https://en.wikipedia.org/api/rest_v1/page/summary/football",
)

def get_sync(url):
    response = requests.get(url)
    print(response.status_code, end=" ", flush=True)

async def get_async(client, url):
    response = await client.get(url, follow_redirects=True)
    print(response.status_code, end=" ", flush=True)

def test_sync():
    start = perf_counter()
    for url in REQUEST_URLS:
        get_sync(url)
    print(f"  Sync elapsed: {perf_counter() - start:.3f}s")

async def test_async():
    start = perf_counter()
    client = httpx.AsyncClient()
    coros = (get_async(client, url) for url in REQUEST_URLS)
    await asyncio.gather(*coros)
    await client.aclose()
    print(f" Async elapsed: {perf_counter() - start:.3f}s")

test_sync()
asyncio.run(test_async())
```

There is much more to the asyncio library, and we just familiarized ourselves with how asynchronous runtime works and the basic syntax in Python. In future posts we will discuss more syntax and use cases with async.

References:
- https://docs.python.org/3/library/asyncio.html
- https://peps.python.org/pep-0492/
