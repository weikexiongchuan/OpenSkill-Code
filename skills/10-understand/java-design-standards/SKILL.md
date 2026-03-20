---
name: java-design-standards
description: Java 技术方案设计规范：模块拆分、公共抽取、API 设计、依赖管理。Use when 做技术设计或架构评审时必读。Do not use when 写具体代码（看 java-dev-standards）。
---

# Java Technical Design Standards

## 模块拆分原则

- 按功能域拆包（`com.app.order` 含 controller+service+repo），不按层拆（不要 `com.app.controllers`）
- 每个模块对外暴露 facade/接口，内部类 package-private
- 一个文件一个 public class

## 公共模块抽取规则

- **三次原则**：两处相似可容忍，三处必须抽取
- 两个模块共享 >3 个 DTO → 抽取 `*-api` 模块（只含接口和模型，零实现）
- 公共工具方法必须无状态、无副作用
- 抽取后原模块通过接口依赖公共模块，禁止反向依赖

## 接口设计

- 面向接口编程：依赖 `List` 不依赖 `ArrayList`
- 入参接受最宽类型（`Collection` 优于 `List`），出参返回最具体类型
- 构造器注入，禁止字段注入和 setter 注入
- 参数超过 5 个 → 引入参数对象

## 对象创建

- >4 个可选字段 → Builder 模式
- 语义化工厂方法优于构造器（`Order.empty()` 优于 `new Order()`）
- DTO 与领域实体分离，禁止 JPA 实体直接出现在 API 响应中

## 依赖管理

- 禁止循环依赖：A 需要 B 且 B 需要 A → 抽取 C 放共享契约
- 组合优于继承，继承深度 <= 2（不含框架类）
- 限界上下文之间用事件通信，上下文内部直接方法调用

## 防御性设计

- 在模块/API 边界校验输入（fail fast），业务逻辑内部不重复校验
- 配置值收敛到配置对象，禁止散落的字符串字面量和魔法数字
- 配置变更走热更新，不需要重启进程

## 设计自查

1. 是否有 God Module（职责 >3 个）
2. 新增代码是否与已有代码重复
3. 公共方法是否放在了正确的模块
4. 模块间是否存在循环依赖
5. 接口是否足够窄（最小暴露原则）
