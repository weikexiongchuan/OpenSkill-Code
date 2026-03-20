---
name: go-dev-standards
description: Go 编码规范：错误处理、goroutine、接口、命名。Use when 写 Go 代码前必读。
---

# Go Development Standards

## 质量红线

| 指标 | 规则 |
|------|------|
| 错误忽略 | 禁止 `_` 忽略 error（除非有注释说明） |
| panic | 生产/库代码禁止 panic，仅 main 初始化可用 |
| 错误比较 | 用 `errors.Is`/`errors.As`，禁止 `==` |
| 类型断言 | 必须用 comma-ok（`v, ok := x.(T)`） |
| goroutine | 每个 goroutine 必须有退出路径 |

## 命名

- Exported `UpperCamelCase`，unexported `lowerCamelCase`
- 缩写全大写：`HTTPClient` 不是 `HttpClient`
- 包名：短、小写、单数、无下划线，包名是调用的一部分（`http.Client` 不是 `http.HTTPClient`）
- 接口：单方法用 `-er` 后缀（`Reader`、`Writer`），保持 1-3 方法
- **接受接口，返回结构体**

## 错误处理

- 每个 error 只处理一次：log 或 return，不要两者都做
- `fmt.Errorf("context: %w", err)` 添加上下文并保留链
- 哨兵错误仅用于调用方需要匹配的条件
- 错误消息：小写，无尾标点

## 并发

- 用 `context.Context` 做取消传播
- `sync.WaitGroup` 或 `errgroup.Group` 跟踪 goroutine 生命周期
- channel 用于通信，mutex 用于状态保护，不要混用
- 零值 `sync.Mutex` 直接可用，禁止拷贝

## 性能

- `strconv` 优于 `fmt` 做基本类型转换
- 已知大小时预分配：`make([]T, 0, n)`
- 避免全局可变状态，用依赖注入
- 提交前跑 `gofmt` + `go vet` + `staticcheck`
