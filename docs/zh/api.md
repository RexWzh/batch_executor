# API 文档

## BatchExecutor 类

批量任务执行器的主要类。

```python
class BatchExecutor:
    def __init__(self, func: Callable[[Any], Any], config: Optional[ExecutorConfig] = None, **kwargs)
```

### 参数

- **func** (`Callable[[Any], Any]`): 要在每个项目上执行的函数
- **config** (`Optional[ExecutorConfig]`): 配置对象（可选）
- **kwargs**: 额外的配置参数，会覆盖 config 中的同名设置

### 方法

#### `run(items: List[Any], mode: str = "auto") -> List[Any]`

自动选择执行模式或使用指定的执行模式。

**参数：**
- **items** (`List[Any]`): 要处理的项目列表
- **mode** (`str`): 执行模式 - "auto", "async", "thread", "process", "hybrid"

**返回：** `List[Any]` - 与输入项目相同顺序的结果列表

#### `async_run(items: List[Any]) -> List[Any]`

异步执行任务（适用于 IO 密集型操作）。

**参数：**
- **items** (`List[Any]`): 要处理的项目列表

**返回：** `List[Any]` - 结果列表

#### `thread_run(items: List[Any]) -> List[Any]`

使用多线程并发执行任务（适用于 IO 密集型操作）。

**参数：**
- **items** (`List[Any]`): 要处理的项目列表

**返回：** `List[Any]` - 结果列表

#### `process_run(items: List[Any]) -> List[Any]`

使用多进程并发执行任务（适用于 CPU 密集型操作）。

**参数：**
- **items** (`List[Any]`): 要处理的项目列表

**返回：** `List[Any]` - 结果列表

#### `hybrid_run(items: List[Any]) -> List[Any]`

使用混合模式执行任务（多个进程 + 每个进程内部使用异步）。

**参数：**
- **items** (`List[Any]`): 要处理的项目列表

**返回：** `List[Any]` - 结果列表

## ExecutorConfig 类

BatchExecutor 的配置类。

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

### 参数

- **nworker** (`Optional[int]`): 工作线程数（线程/进程/异步任务）。默认为 CPU 数量
- **pool_size** (`Optional[int]`): 混合模式中的进程数。默认为 CPU 数量
- **timeout** (`Optional[Union[int, float]]`): 每个任务的超时时间（秒）
- **task_desc** (`str`): 进度条描述
- **logger** (`Optional[logging.Logger]`): 自定义日志记录器实例
- **disable_logger** (`bool)`): 是否禁用日志记录
- **keep_order** (`bool)`): 是否保持结果顺序

## 工具函数

### `batch_async_executor()`

向后兼容的异步执行器函数。

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

向后兼容的线程执行器函数。

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

## 示例

### 基本配置

```python
from batch_executor import BatchExecutor, ExecutorConfig

config = ExecutorConfig(
    nworker=8,
    timeout=30,
    task_desc="处理数据",
    keep_order=True
)

executor = BatchExecutor(your_function, config)
```

### 自定义日志记录器

```python
import logging
from batch_executor import BatchExecutor

logger = logging.getLogger("my_logger")
logger.setLevel(logging.INFO)

executor = BatchExecutor(
    your_function,
    logger=logger,
    task_desc="自定义处理"
)
```

### 错误处理

```python
def safe_function(item):
    try:
        return risky_operation(item)
    except Exception as e:
        return None  # 失败项目返回 None

executor = BatchExecutor(safe_function, timeout=10)
results = executor.run(items)
# 失败的项目在结果列表中将为 None
```