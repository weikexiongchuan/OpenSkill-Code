---
name: rust-dev-standards
description: Rust 编码规范：所有权、错误处理、trait 设计、unsafe 规则。Use when 写 Rust 代码前必读。
---

# Rust Development Standards

## 质量红线

| 指标 | 规则 |
|------|------|
| unsafe | 禁止无 `// SAFETY:` 注释的 unsafe 块 |
| unwrap | 库代码/生产路径禁止 `.unwrap()`，用 `?` 或 `expect("reason")` |
| clone | 禁止为满足借用检查器而 clone，重构所有权 |
| 文档 | 所有 pub 项必须有 doc comment |
| clippy | `cargo clippy` 零警告 |

## 所有权

- 默认借用（`&T`），仅在需要存储/消费时取所有权
- 参数用 `&str` 优于 `&String`，`&[T]` 优于 `&Vec<T>`
- 有时分配有时借用 → `Cow<'_, str>`
- 和借用检查器打架 → 重构数据结构（拆 struct、用索引代替引用），再考虑 `Rc`/`RefCell`
- 生命周期标注：仅在编译器要求时添加

## 错误处理

- `Result<T, E>` 处理可恢复错误，`panic!` 仅用于编程 bug
- 库用 `thiserror` 定义错误枚举，应用用 `anyhow`
- `?` 传播 + `.map_err()` 或 `anyhow::Context` 加上下文

## 命名

- 类型 `UpperCamelCase`，函数/变量 `snake_case`，常量 `SCREAMING_SNAKE_CASE`
- 转换方法：`as_`（廉价借用）、`to_`（昂贵新值）、`into_`（消费 self）
- 构造：`from_`（不可失败）、`try_from_`（可失败）

## Trait

- 保持小而可组合
- 泛型 bounds（`impl Trait`）优先，除非必须动态分发才用 `dyn Trait`
- 尽量 derive：`Display`、`Debug`、`Clone`、`Default`

## 反模式

- 到处 `String` 而 `&str` 就够
- `Arc<Mutex<T>>` 作为第一选择（先考虑 channel）
- 忽略 `#[must_use]` 返回值
- collect 成 Vec 再迭代（链式 lazy 调用）
