---
name: rust-test-standards
description: Rust 测试规范：内置 #[test] + mockall，编译器即测试。Use when 写 Rust 测试前必读。
---

# Rust Testing Standards

## 推荐栈

**内置 `#[test]` + `#[cfg(test)]` + mockall（需要 mock 时）**

选择理由：零依赖覆盖 80% 场景，编译器在运行前就捕获大多数错误。

## 质量红线

| 指标 | 规则 |
|------|------|
| 单元测试位置 | `#[cfg(test)] mod tests` 同文件内 |
| 集成测试位置 | `tests/` 目录，测公共 API |
| `#[cfg(test)]` | 测试模块必须标注，否则编入 release |

## 测试组织

| 类型 | 位置 | 访问权限 |
|------|------|---------|
| 单元测试 | `src/*.rs` 内 `mod tests` | 可访问私有函数 |
| 集成测试 | `tests/*.rs` | 只访问公共 API |
| Doc 测试 | `///` 注释中 | 验证文档示例 |

## 断言

```rust
assert_eq!(actual, expected);           // 相等
assert_ne!(actual, unexpected);         // 不等
assert!(condition, "message: {}", ctx); // 布尔
```

- `#[should_panic(expected = "message")]` 测 panic 路径

## Mock（mockall）

```rust
use mockall::automock;

#[automock]
trait Database {
    fn get(&self, id: u64) -> Option<String>;
}

#[test]
fn test_with_mock() {
    let mut mock = MockDatabase::new();
    mock.expect_get()
        .with(mockall::predicate::eq(1))
        .returning(|_| Some("Alice".into()));
    assert_eq!(mock.get(1), Some("Alice".into()));
}
```

## Mock 使用原则

- **优先用 test struct 实现 trait**，trait 简单时不需要 mockall
- mockall 仅用于需要验证调用次数/参数值时
- 只 mock 外部边界

## AI 常见陷阱

| 陷阱 | 正确做法 |
|------|---------|
| 单元测试放 `tests/` 目录 | 放 `#[cfg(test)] mod tests` 同文件 |
| 忘记 `#[cfg(test)]` | 测试模块必须标注 |
| 简单 trait 也用 mockall | 优先手写 test struct |
| `assert_eq!` 参数无 `Debug` | 被比较类型必须 `#[derive(Debug, PartialEq)]` |
