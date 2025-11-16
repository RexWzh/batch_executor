# Quick Start Guide

This guide will help you get started with the Batch Executor library.

## Installation

```bash
pip install batch-executor
```

## Basic Usage

### Thread-based Execution

```python
from batch_executor import BatchExecutor

def process_data(item):
    # Your processing logic here
    return item.upper()

# Create executor
executor = BatchExecutor(process_data, nworker=4)

# Process items
items = ["hello", "world", "python"]
results = executor.thread_run(items)
print(results)  # ['HELLO', 'WORLD', 'PYTHON']
```

### Async Execution

```python
import asyncio
from batch_executor import BatchExecutor

async def async_process(item):
    # Simulate async operation
    await asyncio.sleep(0.1)
    return item * 2

executor = BatchExecutor(async_process, nworker=10)
results = asyncio.run(executor.async_run([1, 2, 3, 4, 5]))
print(results)  # [2, 4, 6, 8, 10]
```

### Process-based Execution

```python
from batch_executor import BatchExecutor

def cpu_intensive_task(n):
    # CPU-intensive calculation
    return sum(i * i for i in range(n))

executor = BatchExecutor(cpu_intensive_task, nworker=4)
results = executor.process_run([1000, 2000, 3000, 4000])
print(results)
```

### Hybrid Execution

```python
import asyncio
from batch_executor import BatchExecutor

async def io_intensive_task(url):
    # Simulate IO-intensive async operation
    await asyncio.sleep(0.1)
    return f"Processed: {url}"

executor = BatchExecutor(io_intensive_task, pool_size=2, nworker=5)
results = executor.hybrid_run(["url1", "url2", "url3", "url4", "url5"])
print(results)
```

## Configuration Options

```python
from batch_executor import BatchExecutor, ExecutorConfig

config = ExecutorConfig(
    nworker=8,           # Number of workers
    timeout=30,          # Timeout in seconds
    task_desc="Processing",  # Progress bar description
    keep_order=True,     # Preserve result order
    disable_logger=False # Enable logging
)

executor = BatchExecutor(your_function, config)
```

## Error Handling

```python
def risky_function(item):
    if item < 0:
        raise ValueError("Negative values not allowed")
    return item * 2

executor = BatchExecutor(risky_function)
results = executor.run([1, -1, 2, -2, 3])
# Results will contain None for failed items
# Errors are logged automatically
```

## Progress Tracking

```python
from batch_executor import BatchExecutor

def slow_function(item):
    import time
    time.sleep(0.1)
    return item * 2

executor = BatchExecutor(
    slow_function,
    task_desc="Processing items",
    nworker=4
)

# Progress bar will show automatically
results = executor.run(range(20))
```