# API Reference

## BatchExecutor Class

The main class for batch task execution.

```python
class BatchExecutor:
    def __init__(self, func: Callable[[Any], Any], config: Optional[ExecutorConfig] = None, **kwargs)
```

### Parameters

- **func** (`Callable[[Any], Any]`): The function to execute on each item
- **config** (`Optional[ExecutorConfig]`): Configuration object (optional)
- **kwargs**: Additional configuration parameters that override config settings

### Methods

#### `run(items: List[Any], mode: str = "auto") -> List[Any]`

Automatically select execution mode or use specified mode.

**Parameters:**
- **items** (`List[Any]`): List of items to process
- **mode** (`str`): Execution mode - "auto", "async", "thread", "process", "hybrid"

**Returns:** `List[Any]` - List of results in the same order as input items

#### `async_run(items: List[Any]) -> List[Any]`

Execute tasks asynchronously (for IO-bound operations).

**Parameters:**
- **items** (`List[Any]`): List of items to process

**Returns:** `List[Any]` - List of results

#### `thread_run(items: List[Any]) -> List[Any]`

Execute tasks using multiple threads (for IO-bound operations).

**Parameters:**
- **items** (`List[Any]`): List of items to process

**Returns:** `List[Any]` - List of results

#### `process_run(items: List[Any]) -> List[Any]`

Execute tasks using multiple processes (for CPU-bound operations).

**Parameters:**
- **items** (`List[Any]`): List of items to process

**Returns:** `List[Any]` - List of results

#### `hybrid_run(items: List[Any]) -> List[Any]`

Execute tasks using hybrid mode (multiple processes + async within each process).

**Parameters:**
- **items** (`List[Any]`): List of items to process

**Returns:** `List[Any]` - List of results

## ExecutorConfig Class

Configuration class for BatchExecutor.

```python
class ExecutorConfig:
    def __init__(
        self,
        nworker: Optional[int] = None,
        pool_size: Optional[int] = None,
        timeout: Optional[Union[int, float]] = None,
        task_desc: str = "",
        logger: Optional[logging.Logger] = None,
        disable_logger: bool = False,
        keep_order: bool = True
    )
```

### Parameters

- **nworker** (`Optional[int]`): Number of workers (threads/processes/async tasks). Defaults to CPU count
- **pool_size** (`Optional[int]`): Number of processes in hybrid mode. Defaults to CPU count
- **timeout** (`Optional[Union[int, float]]`): Timeout in seconds for each task
- **task_desc** (`str`): Description for progress bar
- **logger** (`Optional[logging.Logger]`): Custom logger instance
- **disable_logger** (`bool)`): Whether to disable logging
- **keep_order** (`bool)`): Whether to preserve result order

## Utility Functions

### `batch_async_executor()`

Backward-compatible async executor function.

```python
def batch_async_executor(
    items: List[Any],
    func_async: Callable[[Any], Coroutine],
    nworker: Optional[int] = None,
    task_desc: str = "",
    logger: Optional[logging.Logger] = None,
    keep_order: bool = True,
    timeout: Optional[Union[int, float]] = None
) -> List[Any]
```

### `batch_thread_executor()`

Backward-compatible thread executor function.

```python
def batch_thread_executor(
    items: List[Any],
    func: Callable[[Any], Any],
    nworker: Optional[int] = None,
    task_desc: str = "",
    logger: Optional[logging.Logger] = None,
    keep_order: bool = True,
    timeout: Optional[Union[int, float]] = None
) -> List[Any]
```

## Examples

### Basic Configuration

```python
from batch_executor import BatchExecutor, ExecutorConfig

config = ExecutorConfig(
    nworker=8,
    timeout=30,
    task_desc="Processing data",
    keep_order=True
)

executor = BatchExecutor(your_function, config)
```

### Custom Logger

```python
import logging
from batch_executor import BatchExecutor

logger = logging.getLogger("my_logger")
logger.setLevel(logging.INFO)

executor = BatchExecutor(
    your_function,
    logger=logger,
    task_desc="Custom processing"
)
```

### Error Handling

```python
def safe_function(item):
    try:
        return risky_operation(item)
    except Exception as e:
        return None  # Return None for failed items

executor = BatchExecutor(safe_function, timeout=10)
results = executor.run(items)
# Failed items will be None in results list
```