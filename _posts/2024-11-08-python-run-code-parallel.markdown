---
layout: post
title: "A Beginner's Guide to Running Python Code in Parallel"
description: "This post will show you how to run Python code in parallel, attached with code examples and explanations."
date: 2024-11-08 13:05:35 +0000
categories: [python]
---

# A Beginner's Guide to Running Python Code in Parallel

In today’s world, where speed and efficiency are paramount, optimizing code to run faster is crucial. One of the most
effective ways to speed up Python code is to run it in parallel. By leveraging parallel processing, you can perform
multiple tasks simultaneously, making your programs significantly faster, especially for CPU-bound or I/O-bound tasks.

This blog post will cover the basics of parallel processing in Python, exploring common approaches like multiprocessing,
concurrent.futures, and asyncio, and highlighting when and why you might want to use each one. Let’s dive in and explore
how you can make your Python code run faster by running tasks in parallel!

## Why Parallel Processing?

Python, while a powerful language, has limitations when it comes to running CPU-bound code. The Global Interpreter
Lock (GIL) in CPython means that only one thread can execute Python bytecode at a time, making threading less effective
for CPU-bound tasks. However, we can bypass this limitation by using parallel processing, which distributes tasks across
multiple processors or cores.

Here are some scenarios where parallel processing can make a significant difference:

* Data Processing: Analyzing large datasets, performing machine learning computations, or applying transformations to
  massive data structures.
* Web Scraping: Scraping multiple web pages simultaneously.
* I/O Operations: Downloading files, reading from disk, or handling network requests in parallel.
  Let’s look at some of the main libraries in Python that facilitate parallel processing.

## Using multiprocessing for Parallel Processing

Python's multiprocessing library is one of the most popular ways to run tasks in parallel. It allows you to create
multiple processes, each with its own Python interpreter, thus bypassing the GIL limitation.

Here's a simple example of using multiprocessing to compute the squares of numbers in parallel:

```python

from multiprocessing import Pool


def square(x):
    return x * x


if __name__ == '__main__':
    with Pool(4) as p:
        results = p.map(square, [1, 2, 3, 4, 5])
    print(results)

```

### Explanation

* Pool of Processes: We create a pool of 4 processes.
* Mapping Tasks: The p.map() function splits the list [1, 2, 3, 4, 5] into chunks and distributes them to each process.
* Result Aggregation: The results from each process are collected and printed.

### Pros and Cons

* Pros: Bypasses the GIL, effective for CPU-bound tasks.
* Cons: Starting processes has overhead, which can be costly for small tasks.

## concurrent.futures: A Higher-Level Interface for Concurrency

The concurrent.futures module provides a higher-level API for parallel execution, making it easier to work with threads
and processes interchangeably. The ProcessPoolExecutor and ThreadPoolExecutor classes offer a streamlined interface for
managing parallel tasks.

Here’s an example using ProcessPoolExecutor:

```python

from concurrent.futures import ProcessPoolExecutor


def cube(x):
    return x ** 3


if __name__ == '__main__':
    with ProcessPoolExecutor(max_workers=4) as executor:
        results = list(executor.map(cube, [1, 2, 3, 4, 5]))
    print(results)

```

### Explanation

* Process Pool Management: We specify the maximum number of processes with max_workers=4.
* Mapping Tasks: The executor.map() function is similar to multiprocessing.Pool, distributing tasks to each worker
  process.

### When to Use concurrent.futures

* When You Need Both Threads and Processes: concurrent.futures is flexible, allowing you to switch between threads and
  processes.
* For a Cleaner API: The API is simple and often cleaner than multiprocessing.

## asyncio for Asynchronous I/O Operations

If your tasks are I/O-bound, asyncio is an excellent alternative. Instead of using multiple threads or processes,
asyncio uses an event loop to manage I/O operations asynchronously, which can be highly efficient for tasks like network
requests or file I/O.

Here’s a quick example using asyncio:

```python
import asyncio


async def fetch_data(url):
    print(f"Fetching data from {url}")
    await asyncio.sleep(1)  # Simulating network delay
    return f"Data from {url}"


async def main():
    urls = ['http://example.com', 'http://example.org', 'http://example.net']
    tasks = [fetch_data(url) for url in urls]
    results = await asyncio.gather(*tasks)
    print(results)


# Run the event loop
asyncio.run(main())

```

### Explanation

* Async Function: fetch_data() is an asynchronous function that simulates a network request.
* Event Loop Execution: asyncio.run(main()) starts the event loop, running all tasks concurrently.

### Pros and Cons

* Pros: Very efficient for I/O-bound tasks, lightweight.
* Cons: Limited to a single thread; not suitable for CPU-bound tasks.

## Choosing the Right Tool for the Job

Each of these approaches has specific strengths and is suited for different types of tasks. Here’s a quick comparison to
help you decide:

| Method              | 	Use Case                  | 	Best For                                        |
|---------------------|----------------------------|--------------------------------------------------|
| multiprocessing     | 	CPU-bound tasks           | 	Tasks requiring multiple cores                  |
| concurrent.futures	 | Flexible concurrency       | 	Both CPU-bound and I/O-bound tasks, simpler API |
| asyncio	            | Single-threaded, async I/O | 	I/O-bound tasks only, lightweight               |

## Final Thoughts
Parallel processing in Python can significantly reduce the runtime of your programs, making them faster and more efficient. Understanding when and how to use each method—whether multiprocessing, concurrent.futures, or asyncio—is crucial for getting the best performance out of your code.

Experiment with these libraries and start optimizing your Python projects. By implementing parallel processing, you’ll be well-equipped to tackle tasks more efficiently and take full advantage of your hardware’s capabilities.


## References
[Python Doc](https://docs.python.org/3/library/multiprocessing.html)