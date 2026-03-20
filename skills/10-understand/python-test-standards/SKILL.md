---
name: python-test-standards
description: Python 测试规范：pytest 推荐栈、fixture 模式、mock 规则。Use when 写 Python 测试前必读。
---

# Python Testing Standards

## 推荐栈

**pytest + pytest-mock + pytest-asyncio**

选择理由：`assert` 直接用无需记方法名，fixture 链式组合，丰富插件生态。

## 质量红线

| 指标 | 规则 |
|------|------|
| 业务逻辑覆盖 | >= 80% |
| 测试间隔离 | 无共享可变状态 |
| 文件操作 | 必须用 `tmp_path` fixture |

## 命名与结构

- 文件：`test_{module}.py`
- 函数：`test_{method}_{condition}_{expected}`
- AAA 模式：Arrange / Act / Assert，空行分隔
- 同逻辑多输入 → `@pytest.mark.parametrize`，至少 3 case

## Fixture

- 可复用 setup → `@pytest.fixture`，不用 `setUp`/`tearDown`
- scope 正确性：`session` scope 不依赖 `function` scope fixture
- 数据库/外部资源 → `yield` fixture + cleanup
- fixture 放 `conftest.py` 按目录共享

## Mock 规则

- 用 `mocker.patch()`（pytest-mock），不手写 `with patch()`
- **patch 位置是 import 处，不是定义处**：`mocker.patch("mymodule.service.requests.get")` 不是 `mocker.patch("requests.get")`
- 只 mock 外部依赖（HTTP/DB/文件系统），不 mock 被测函数
- 优先用 fake/stub，mock 仅用于验证调用

## 异步测试

- 标记 `@pytest.mark.asyncio`
- 用 `pytest-asyncio` 的 `async def test_*`
- 异步 fixture 同样用 `@pytest.fixture` + `async def`

## AI 常见陷阱

| 陷阱 | 正确做法 |
|------|---------|
| patch 位置搞反 | patch import 处而非定义处 |
| session fixture 依赖 function fixture | 同 scope 或更宽 scope |
| 文件写到工作目录 | 用 `tmp_path` fixture |
| 忘记 `await` 异步断言 | `result = await func(); assert result == ...` |
