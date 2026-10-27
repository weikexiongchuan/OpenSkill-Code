---
name: java-test-standards
description: Java 测试编写规范：命名、结构、隔离、覆盖率。Use when 写 Java 单元测试或集成测试时必读。Do not use when 写业务代码。
---

# Java Testing Standards

## 推荐栈

**JUnit 5 + Mockito + AssertJ**

选择理由：注解驱动自文档化，`when().thenReturn()` 读如英语，AssertJ 链式断言一个模式走天下。

## 命名

- 格式：`methodName_condition_expectedResult`
- 示例：`calculateTotal_emptyCart_returnsZero`
- 描述行为不描述实现："should reject expired coupon" 不是 "testValidateCoupon3"

## 结构

- 每个测试遵循 AAA：Arrange / Act / Assert，空行分隔
- 一个测试一个逻辑断言
- 一个生产类对应一个测试类，同包不同 source root

## 隔离

- 测试之间无共享可变状态，用 `@BeforeEach` 做 setup
- 每个测试独立且幂等

## Mock 规则

- Mock 外部依赖（DB、HTTP、MQ），不 mock 被测类和简单值对象
- 优先用 fake/stub（提供固定数据），mock 只用于验证交互
- 测试 setup 不超过 10 行，复杂对象用 Test Data Builder

## 参数化

- 同一逻辑多输入 → `@ParameterizedTest`
- 按实际变化选择有价值的 case，不设固定数量

## 覆盖率

- 按 [最小验证原则](../../../docs/testing-scope.md) 覆盖关键业务变化，不以统一行覆盖率作为交付门槛
- 基础设施/胶水代码不强求覆盖率

## 要测什么

| 测 | 不测 |
|----|------|
| 业务规则 | getter/setter |
| 边界值（null、空、上下限） | 框架代码 |
| 错误处理路径 | private 方法（通过 public 间接覆盖） |
| 状态转换 | 简单委托 |

## 集成测试

- 用 Testcontainers 启动真实 DB
- 标记 `@Tag("integration")`，独立于单元测试运行

## Flaky 测试

- 发现即隔离，48 小时内修复或删除

## AI 常见陷阱

| 陷阱 | 正确做法 |
|------|---------|
| 忘记 `@ExtendWith(MockitoExtension.class)` | `@Mock` 不生效，NPE 但测试可能误过 |
| `any()` 和字面量混用 | 全 matcher 或全字面量：`any(), eq("x")` |
| mock final 类加 PowerMock | Mockito 5+ 默认支持，不需要额外配置 |
| AssertJ 链中忘记实际断言 | `assertThat(x)` 后必须跟 `.isXxx()` |
