---
name: go-test-standards
description: Go 测试规范：标准 testing + testify，表驱动模式。Use when 写 Go 测试前必读。
---

# Go Testing Standards

## 推荐栈

**标准 `testing` 包 + testify（assert/require/mock）**

选择理由：表驱动模式高度一致，testify 消除 `if got != want` 样板。

## 质量红线

| 指标 | 规则 |
|------|------|
| 业务逻辑覆盖 | >= 80% |
| race 检测 | 必须 `go test -race` 通过 |
| 测试隔离 | `t.Parallel()` 时无共享可变状态 |

## 表驱动测试（标准模式）

```go
func TestFoo(t *testing.T) {
    tests := []struct {
        name     string
        input    int
        expected int
    }{
        {"normal", 1, 2},
        {"zero", 0, 0},
        {"negative", -1, -2},
    }
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            assert.Equal(t, tt.expected, Foo(tt.input))
        })
    }
}
```

## assert vs require

| 用途 | 选择 | 行为 |
|------|------|------|
| 前置条件 | `require` | 失败立即停止当前测试 |
| 实际断言 | `assert` | 失败继续执行，报告所有失败 |

## Mock 规则

- 优先用接口 + 简单 test struct，不过度依赖 mock 框架
- 需要验证调用次数/参数时用 `testify/mock`
- mock 仅用于外部边界（HTTP/DB/MQ）

## Helper 函数

- 第一行必须 `t.Helper()`，否则错误指向 helper 而非调用者
- 返回 cleanup → 用 `t.Cleanup(func() { ... })`

## AI 常见陷阱

| 陷阱 | 正确做法 |
|------|---------|
| `assert.Equal(t, actual, expected)` 参数顺序反 | `assert.Equal(t, expected, actual)` |
| `t.Parallel()` + 共享状态 | 并行时确保完全隔离 |
| 忘记 `t.Helper()` | helper 函数第一行加 |
| 表驱动 loop 变量捕获 | Go 1.22+ 已修复，旧版本用 `tt := tt` |
