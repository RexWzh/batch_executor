# Batch Executor Documentation

A powerful Python library for executing batch tasks with multiple concurrency modes: async, threading, multiprocessing, and hybrid execution.

## Features

- **Multiple Execution Modes**: Support for async, threading, multiprocessing, and hybrid execution
- **Progress Tracking**: Built-in progress bars with tqdm integration
- **Timeout Control**: Configurable timeout for individual tasks
- **Error Handling**: Robust error handling and logging
- **Order Preservation**: Optional result order preservation
- **Flexible Configuration**: Comprehensive configuration options

## Quick Start

```python
from batch_executor import BatchExecutor

# Simple usage
def process_item(item):
    return item * 2

executor = BatchExecutor(process_item)
results = executor.run([1, 2, 3, 4, 5])
print(results)  # [2, 4, 6, 8, 10]
```

## Installation

```bash
pip install batch-executor
```

## Documentation

- [Quick Start Guide](quickstart.md) - Get started quickly
- [API Reference](api.md) - Detailed API documentation

## License

MIT License - see the [LICENSE](../LICENSE) file for details.