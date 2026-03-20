---
name: python-dev-standards
description: Python 编码规范：风格、类型、异步、数据处理。Use when 写 Python 代码前必读。
---

# Python Development Standards

## 质量红线

| 指标 | 上限 |
|------|------|
| 函数行数 | <= 30 行逻辑 |
| 行宽 | 88 字符（Black 默认） |
| 嵌套深度 | <= 3 层 |

## 风格

- PEP 8：4 空格缩进，`snake_case` 函数/变量，`PascalCase` 类
- f-string 优于 `.format()` 和 `%`
- `pathlib.Path` 优于 `os.path`
- `enum.Enum` 代替散落的字符串常量

## 类型

- 所有函数签名必须有类型标注：`def process(items: list[str]) -> dict[str, int]:`
- 3.10+ 用 `X | None` 代替 `Optional[X]`
- 结构化数据用 `dataclass` 或 `pydantic.BaseModel`，不用裸 dict

## 空值与默认

- 禁止可变默认参数（`def f(x=[])`），用 `None` + 内部初始化
- 集合不返回 None，返回空集合

## 异常

- 抛具体异常（`ValueError`、`KeyError`），不抛泛 `Exception`
- 领域错误定义自定义异常类
- 用 `logging` 模块，生产代码禁止 `print()`

## 异步

- `async` 函数内禁止调用阻塞 IO（`requests`、`time.sleep`）
- 用 `httpx` / `aiohttp` + `asyncio.sleep`
- 并发 IO 用 `asyncio.gather()`，用 `Semaphore` 限制并发数

## 数据处理（pandas/numpy）

- **向量化优先**：禁止 `for index, row in df.iterrows()`
- 链式操作用 `.pipe()`，避免中间变量泥潭
- numpy 预分配数组，禁止循环 `np.append()`
- 用 broadcasting 代替显式循环

## 资源管理

- 所有资源用 `with` 上下文管理器
- import 顺序：标准库 → 第三方 → 本地，空行分隔，仅绝对导入
