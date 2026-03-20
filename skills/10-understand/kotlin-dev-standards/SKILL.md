---
name: kotlin-dev-standards
description: Kotlin 编码规范：空安全、协程、密封类、Java 互操作。Use when 写 Kotlin 代码前必读。
---

# Kotlin Development Standards

## 质量红线

| 指标 | 规则 |
|------|------|
| `!!` | 生产代码禁止非空断言，仅测试可用 |
| GlobalScope | 禁止 `GlobalScope.launch`，必须用 scoped |
| 协程阻塞 | 协程内禁止 `Thread.sleep` / 阻塞IO，用 `withContext(Dispatchers.IO)` |
| val/var | 默认 `val`，仅必要时 `var` |
| async | `async` 必须配 `.await()`，否则异常被吞 |

## 空安全

- `val` 优于 `var`，不可变集合优于可变
- Java 平台类型视为 nullable，在边界加 `@Nullable`/`@NonNull`
- 用 `?.let {}`、`?:`（elvis）、`as?`（安全转型）
- 需要非空契约用 `requireNotNull()` / `checkNotNull()`，不用 `!!`

## 协程

- 结构化并发：在 scoped 的 `CoroutineScope` 中启动
- suspend 函数内用 `coroutineScope {}` 启动子协程
- 一个子协程失败不应取消兄弟 → `supervisorScope {}`
- 阻塞调用 → `withContext(Dispatchers.IO)`
- `CoroutineExceptionHandler` 仅顶层使用

## 数据建模

- 值对象 → `data class`（不可变），不继承 data class
- 穷举状态 → `sealed class`/`sealed interface` + `when`
- 无共享状态时 `sealed interface` 优于 `sealed class`

## 惯用法

- 扩展函数用于不拥有的类型，限制可见性（`private`/`internal`）
- 无副作用 + 廉价 + 确定性 → property 优于 function
- 短 lambda 用 `it`，嵌套 lambda 必须命名参数

## Java 互操作

- Java 调用方 → 加 `@JvmStatic`、`@JvmField`、`@JvmOverloads`
- suspend 函数被 Java 调用 → `@Throws`
- companion 常量：基本类型 `const val`，复杂类型 `@JvmField val`
