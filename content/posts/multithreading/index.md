---
title: "Multi-Threading"
date: 2026-11-26
draft: false
description: "A practical introduction to processes, threads, synchronization, and thread pools in Python."
categories: ["Python"]
tags: ["Python"]
readingTime: true
---

## Process and thread: the distinction

A **process** is an executing program with its own address space and operating-system resources. A **thread** is an execution path inside a process. Threads belonging to one process share its memory, while each thread has its own call stack and execution state.

This shared-memory model makes communication convenient, but it also means that concurrent code must be designed carefully.

## When is multithreading useful?

Threads are especially useful for I/O-bound work: waiting for network responses, reading files, or communicating with a database. While one thread is waiting, another can make progress. Threads do not automatically make every program faster; CPU-heavy Python work may be limited by the Global Interpreter Lock (GIL) in standard CPython builds. For CPU-bound workloads, multiprocessing or native libraries may be more suitable.

## Creating and joining threads

Python provides the `threading` module. A `Thread` receives a target function and optional arguments. Calling `start()` schedules the thread; calling `join()` makes the caller wait for it to finish.

```python
import threading
import time

def worker(label):
    print(f"{label} started")
    time.sleep(1)
    print(f"{label} finished")

threads = [threading.Thread(target=worker, args=(name,))
           for name in ("A", "B", "C")]

for thread in threads:
    thread.start()

for thread in threads:
    thread.join()

print("All workers have completed")
```

The order of messages from different threads is not guaranteed. The scheduler decides when each thread runs.

## Shared data, race conditions, and locks

A **race condition** occurs when a result depends on the timing of concurrent operations. For example, two threads updating the same counter can overwrite one another's changes. A `threading.Lock` protects a critical section so that only one thread at a time can execute it.

```python
counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(10_000):
        with lock:
            counter += 1
```

Using `with lock:` ensures the lock is released when the block exits, including when an exception occurs. Keep locked sections small and avoid acquiring multiple locks in inconsistent orders.

## ThreadPoolExecutor

For batches of similar tasks, `concurrent.futures.ThreadPoolExecutor` is often simpler than manually creating threads. It manages a pool and provides methods such as `map()` and `submit()`.

```python
from concurrent.futures import ThreadPoolExecutor

def square(number):
    return number * number

with ThreadPoolExecutor(max_workers=4) as pool:
    results = list(pool.map(square, [1, 2, 3, 4, 5]))

print(results)
```

## Practical checklist

- Use threads mainly when tasks spend time waiting for I/O.
- Protect shared mutable state with synchronization primitives.
- Join threads or manage them through an executor before relying on their results.
- Do not assume a particular execution or print order.
- Measure performance; concurrency adds coordination overhead.

Multithreading is a tool for structuring concurrent work, not a universal speed switch. Choose it after understanding the workload and the data shared between tasks.
