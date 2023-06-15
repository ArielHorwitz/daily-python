# Async Iterators and Context Managers
> *This post follows [Asyncio Basics](/tips/022-asyncio-basics.md).*

Since we have covered iterators, context managers, and asyncio, it is only fair that we discuss their combination. As with normal iterators, we can create asynchronous iterators by defining `__aiter__` and `__anext__` (or simply using the `yield` keyword in a coroutine). Asynchronous context managers can be defined using `__aenter__` and `__aexit__` (or simply using the `asynccontextmanager` decorator from `contextlib`). We can use these protocols by prepending `async` to the `for` and `with` keywords:
```python
import asyncio
from contextlib import asynccontextmanager

@asynccontextmanager
async def async_context_manager():
    try:
        print("context set")
        yield "context"
    finally:
        print("context cleared")

async def async_iterator():
    yield "first"
    yield "second"

async def main():
    async with async_context_manager() as ctx:
        async for i in async_iterator():
            print(ctx, i)

asyncio.run(main())
```

As you may notice, the protocols are very similar to their synchronous counterparts. A question arises: why do we need the dedicated `async with` and `async for`? As we know, synchronous functions cannot call coroutines, and the normal protocols using `__iter__`, `__next__`, `__enter__`, and `__exit__` are defined as normal methods. Using `for` and `with` keywords each involve calling multiple methods. To allow these methods to be asynchronous, we must follow another (but similar) protocol which requires it's own asynchronous methods and invoke them with dedicated syntax.

I think it's interesting to note that both iterators and context managers can be expressed as generators, and in previous posts we have learned of the conceptual connection between generators and coroutines. In a future post we will discuss asyncio `Tasks` and the functionality they provide to manage coroutines. Stay tuned!

References:
- https://peps.python.org/pep-0492/
- https://peps.python.org/pep-0525/
