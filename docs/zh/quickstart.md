# 快速开始指南

本指南将帮助您快速开始使用 Batch Executor 库。

## 安装

```bash
pip install batch-executor
```

## 基本用法

### 基于线程的执行

```python
from batch_executor import BatchExecutor

def process_data(item):
    # 您的处理逻辑在这里
    return item.upper()

# 创建执行器
executor = BatchExecutor(process_data, nworker=4)

# 处理项目
items = ["hello", "world", "python"]
results = executor.thread_run(items)
print(results)  # ['HELLO', 'WORLD', 'PYTHON']
```

### 异步执行

```python
import asyncio
from batch_executor import BatchExecutor

async def async_process(item):
    # 模拟异步操作
    await asyncio.sleep(0.1)
    return item * 2

executor = BatchExecutor(async_process, nworker=10)
results = asyncio.run(executor.async_run([1, 2, 3, 4, 5]))
print(results)  # [2, 4, 6, 8, 10]
```

### 基于进程的执行

```python
from batch_executor import BatchExecutor

def cpu_intensive_task(n):
    # CPU 密集型计算
    return sum(i * i for i in range(n))

executor = BatchExecutor(cpu_intensive_task, nworker=4)
results = executor.process_run([1000, 2000, 3000, 4000])
print(results)
```

### 混合执行

```python
import asyncio
from batch_executor import BatchExecutor

async def io_intensive_task(url):
    # 模拟 IO 密集型异步操作
    await asyncio.sleep(0.1)
    return f"已处理: {url}"

executor = BatchExecutor(io_intensive_task, pool_size=2, nworker=5)
results = executor.hybrid_run(["url1", "url2", "url3", "url4", "url5"])
print(results)
```

## 配置选项

```python
from batch_executor import BatchExecutor, ExecutorConfig

config = ExecutorConfig(
    nworker=8,           # 工作线程数
    timeout=30,          # 超时时间（秒）
    task_desc="处理中",  # 进度条描述
    keep_order=True,     # 保持结果顺序
    disable_logger=False # 启用日志记录
)

executor = BatchExecutor(your_function, config)
```

## 错误处理

```python
def risky_function(item):
    if item < 0:
        raise ValueError("不允许负值")
    return item * 2

executor = BatchExecutor(risky_function)
results = executor.run([1, -1, 2, -2, 3])
# 失败的项目结果将为 None
# 错误会自动记录
```

## 进度跟踪

```python
from batch_executor import BatchExecutor

def slow_function(item):
    import time
    time.sleep(0.1)
    return item * 2

executor = BatchExecutor(
    slow_function,
    task_desc="处理项目中",
    nworker=4
)

# 进度条会自动显示
results = executor.run(range(20))
```