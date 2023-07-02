# Asyncio Tasks
> This post follows [Asyncio Basics](/tips/022-asyncio-basics.md).

So far we have learned the syntax and some basic concepts of asynchronous concurrency. We will now discuss making use of the asyncio library proper, in particular the use of `asyncio.Task` to schedule concurrent coroutines.

While a coroutine can be awaited from another coroutine, scheduling multiple coroutines requires creating a `Task` for each of them, which is then handled by the event loop. Creating `Tasks` is usually done with `asyncio.create_task`:

```python
import asyncio

async def mycoro(arg: int):
    ...

# Schedule 3 tasks to run concurrently
async def concurrent_tasks():
    task1 = asyncio.create_task(mycoro())
    task2 = asyncio.create_task(mycoro())
    task3 = asyncio.create_task(mycoro())
    print("started 3 tasks")
    await asyncio.gather(task1, task2, task3)
    print("completed 3 tasks")

# In Python 3.11
async def concurrent_tasks_311():
    async with asyncio.TaskGroup() as tg:
        tg.create_task(mycoro())
        tg.create_task(mycoro())
        tg.create_task(mycoro())
        print("starting task group")
    print("completed task group")
```

Once a task has been created, the event loop will execute it asynchronously. Tasks are awaitable, but can also be managed: gathered, timed out, cancelled, trigger callbacks, and more.
```python
def on_task_done(task):
    assert task.done()
    print(task.get_name(), task.result())


async def manage_tasks():
    task1 = asyncio.create_task(mycoro())
    task2 = asyncio.create_task(mycoro())
    print("started 2 tasks")
    print(asyncio.all_tasks())
    task1.add_done_callback(on_task_done)
    assert not task1.done()
    for coro in asyncio.as_completed(task1, task2):
        next_result = await coro
    print("completed 2 tasks")
```

Another interesting object is the lower level `asyncio.Future`. It is very similar to a task, except that it does not wrap a coroutine and it's result can be set manually:
```python
async def run_server(self):
    self.serving = asyncio.Future()
    self.serving.add_done_callback(print)
    await self.serving
    print(self.serving.result())

def some_handler(self):
    self.serving.set_result("Shutdown")
```

When using asyncio extensively, please refer to the documentation in the references below as there is more functionality and details not discussed here. This covers the basics of using asyncio.

References:
- https://docs.python.org/3/library/asyncio.html
- https://docs.python.org/3/library/asyncio-task.html
- https://docs.python.org/3/library/asyncio-future.html
