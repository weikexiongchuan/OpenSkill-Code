---
name: java-dev-standards
description: Java 编码质量红线与日常规范。Use when 写 Java 代码前必读。Do not use when 做技术方案设计（看 java-design-standards）或写测试（看 java-test-standards）。
---

# Java Development Standards

## 质量红线

| 指标 | 上限 |
|------|------|
| 方法行数 | <= 40 行（不含空行和注释） |
| 圈复杂度 | <= 5 |
| 嵌套深度 | <= 3 层 |
| 类行数 | <= 300 行 |
| 构造器参数 | <= 5 个 |

## 命名

- 类：UpperCamelCase 名词（`OrderService`）
- 方法：lowerCamelCase 动词（`calculateTotal`）
- 常量：`UPPER_SNAKE_CASE`
- 布尔变量/方法：`is`/`has`/`should` 前缀
- 不缩写，除非通用（`id`、`url`）

## 结构

- 早返回（guard clause）代替嵌套 if-else
- 一行一语句，不用分号连接多语句
- 显式 import，禁止通配符 `import java.util.*`
- 布尔参数是坏味道，用枚举或拆方法

## 异常

- 捕获具体异常，禁止裸 `catch (Exception e)`（除非重新抛出）
- 禁止吞异常（空 catch）
- try-with-resources 管理资源，禁止手写 finally 关闭
- 错误必须显式抛出或记录，禁止用默认值、回退分支、降级逻辑掩盖问题
- 启动失败就 fail fast，执行失败就明确报错，禁止静默继续
- 运行时异常必须打印 `error` 日志，包含异常堆栈和必要业务上下文
- 异常对外抛出必须遵守安全规范：底层敏感异常不直接透传，先记录日志，再包装成安全异常
- 公共异常优先收敛到项目级异常基类，常见异常按领域封装，不直接把底层实现细节暴露给上层
- 例如 `FileNotFoundException` 这类可能泄漏路径/目录结构的异常，不直接对外抛原始信息；日志记录原始异常，对外抛出只说明“这里发生了文件读取异常”这类安全描述

## 空值

- 集合方法不返回 null，返回空集合
- 可能缺席的返回值用 `Optional`
- `Optional` 不做字段、不做参数

## 并发

- 热路径禁止 `synchronized`，用 `ConcurrentHashMap` / 原子类 / 无锁结构
- Virtual Threads 场景：用 `ReentrantLock` 代替 `synchronized`（避免 pin）
- 禁止双重检查锁（除非 volatile + 充分理由）

## 日志

- 参数化消息：`log.info("Processing {}", orderId)`，禁止字符串拼接
- 必须包含关键上下文（业务 ID、规则名、策略 ID）
- 生产代码禁止 `System.out.println`

## 不可变优先

- 字段默认 `final`
- 值类型用 record（Java 16+）
- `equals`/`hashCode` 必须成对重写，用 `Objects.hash()`

## 反模式清单

- God Class（>300 行）
- 有状态的工具类
- Service Locator
- SELECT *
- 反射（热路径）
- 魔法数字/字符串
