# Batch Executor 文档

一个强大的 Python 库，支持多种并发模式执行批量任务：异步、多线程、多进程和混合执行。

## 功能特性

- **多种执行模式**：支持异步、多线程、多进程和混合执行
- **进度跟踪**：内置进度条，集成 tqdm
- **超时控制**：可配置的任务超时时间
- **错误处理**：强大的错误处理和日志记录
- **顺序保持**：可选的结果顺序保持
- **灵活配置**：全面的配置选项

## 快速开始

```python
from batch_executor import BatchExecutor

# 简单使用
def process_item(item):
    return item * 2

executor = BatchExecutor(process_item)
results = executor.run([1, 2, 3, 4, 5])
print(results)  # [2, 4, 6, 8, 10]
```

## 安装

```bash
pip install batch-executor
```

## 文档

- [快速开始指南](zh/quickstart.md) - 快速上手
- [API 文档](zh/api.md) - 详细的 API 文档

## 许可证

MIT 许可证 - 详见 [LICENSE](../LICENSE) 文件。