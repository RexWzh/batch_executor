# batch_executor

[![PyPI version](https://img.shields.io/pypi/v/batch_executor.svg)](https://pypi.python.org/pypi/batch_executor)
[![Travis CI](https://img.shields.io/travis/RexWzh/batch_executor.svg)](https://travis-ci.com/RexWzh/batch_executor)
[![Documentation Status](https://img.shields.io/badge/docs-mkdocs-blue)](https://RexWzh.github.io/batch_executor/)

A high-performance batch executor supporting async, threading, multiprocessing, and hybrid execution modes with progress bars and logging.

* Free software: MIT license
* Documentation: [https://RexWzh.github.io/batch_executor/](https://RexWzh.github.io/batch_executor/)

## Features

- **Multiple Execution Modes**: Support for async, threading, multiprocessing, and hybrid execution
- **Progress Tracking**: Built-in progress bars with tqdm integration  
- **Timeout Control**: Configurable timeout for individual tasks
- **Error Handling**: Robust error handling and logging
- **Order Preservation**: Optional result order preservation
- **Flexible Configuration**: Comprehensive configuration options
- **Bilingual Support**: Documentation available in English and Chinese

## Quick Start

### Installation

```bash
pip install batch-executor
```

### Basic Usage

```python
from batch_executor import BatchExecutor

# Simple usage
def process_item(item):
    return item * 2

executor = BatchExecutor(process_item)
results = executor.run([1, 2, 3, 4, 5])
print(results)  # [2, 4, 6, 8, 10]
```

### Advanced Usage

```python
from batch_executor import BatchExecutor, ExecutorConfig

# With configuration
config = ExecutorConfig(
    nworker=8,           # Number of workers
    timeout=30,          # Timeout in seconds
    task_desc="Processing",  # Progress bar description
    keep_order=True,     # Preserve result order
)

executor = BatchExecutor(your_function, config)
results = executor.run(items, mode="thread")  # or "async", "process", "hybrid"
```

## Execution Modes

- **Async**: For IO-bound operations using asyncio
- **Thread**: For IO-bound operations using threading
- **Process**: For CPU-bound operations using multiprocessing  
- **Hybrid**: For large-scale IO operations using multiprocessing + asyncio
- **Auto**: Automatically selects the best mode based on your function

## Documentation

For detailed documentation, examples, and API reference, visit: [https://RexWzh.github.io/batch_executor/](https://RexWzh.github.io/batch_executor/)

## License

MIT license. See [LICENSE](LICENSE) file for details.

## Credits

This package was created with [Cookiecutter](https://github.com/audreyr/cookiecutter) and the [audreyr/cookiecutter-pypackage](https://github.com/audreyr/cookiecutter-pypackage) project template.