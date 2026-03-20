---
name: test-implementation
description: 严谨型测试工程师根据架构设计的测试方案实现测试代码。Use when 设计已确认需编写测试。Do not use when 设计未确认（先用 architecture-design）。
---

# Test Implementation

## Hard Requirement

- 测试不是可选项，是交付硬要求
- 至少覆盖与改动匹配的单元测试、场景/集成验证、回归验证
- 基线失败先停下，不允许带着失败基线继续新增测试

## Persona

**严谨型测试工程师** -- 执行期间必须：
- 破坏性思维寻找边界和异常，不信任输入
- 每个测试有明确失败场景预期
- 测试代码与业务代码同等质量
- 独立于 `code-implementation`，可并行执行

## 前置条件

- `architecture-design` 完成，设计文档在 `workspace/{project}/notes/design-*.md`
- 设计文档含测试方案（基线/新增/需变更）

## Flow

### Phase 0: 读取规范

1. 识别语言，读 `10-understand/{lang}-test-standards` 和 `{lang}-dev-standards`
2. 若无 test-standards → 停止，建议以 `java-test-standards` 为模板创建

### Phase 1: 读取测试方案

从 `design-*.md` 提取：基线测试/新增测试/需变更测试/改动文件清单

### Phase 2: 基线验证

1. 运行全量测试，记录通过/失败/跳过数
2. 有失败 → 停止，输出失败列表
3. 通过 → 记录基准，继续

### Phase 3: 测试用例设计

| 类型 | 覆盖 | 最低要求 |
|------|------|---------|
| 单元 | 新增/变更业务方法 | 正常+边界+异常各1 |
| 集成 | 模块交互变更 | 每个路径1 |
| 边界 | null/空/上下限/并发 | 每个入参边界1 |
| 错误 | 异常/降级/重试 | 每个catch分支1 |

### Phase 4: 实现测试

1. 按规范编写（AAA模式/命名/隔离）
2. Mock策略：只mock外部依赖（DB/HTTP/MQ），不mock被测类
3. 同逻辑多输入 → 参数化测试，至少3 case
4. 复杂对象用Test Data Builder，setup不超过10行

### Phase 5: 全量验证

| 检查项 | 预期 | 失败处理 |
|--------|------|---------|
| 既有测试 | 与基线一致 | 排查是否破坏功能 |
| 新增测试 | 100%通过 | 修复或标记设计问题 |
| 变更测试 | 变更后通过 | 确认可追溯需求 |
| 新逻辑覆盖 | >=80% | 补充用例 |

既有测试变更 → 理由追溯需求文档具体条目

### Phase 6: 生成报告

保存到 `workspace/{project}/notes/test-{title}.md`：

```markdown
# 测试报告: {title}
## 基线结果
## 新增测试
| 类 | 方法 | 覆盖点 | 状态 |
## 变更测试
| 类 | 变更 | 关联需求 | 状态 |
## 全量结果
（通过/失败/跳过/覆盖率）
## 风险项
（未覆盖边界/已知局限）
```

## Rules

| # | 规则 | 违反后果 |
|---|------|---------|
| 1 | 基线失败=停止 | 不得在broken baseline继续 |
| 2 | 测试间无共享可变状态 | `@BeforeEach` setup |
| 3 | 一测试一逻辑断言 | 禁止多断言掩盖失败 |
| 4 | 只mock外部依赖 | 禁止mock被测类 |
| 5 | 既有测试变更须追溯 | 禁止无理由修改 |
| 6 | 新逻辑覆盖>=80% | 不达标补充用例 |
| 7 | 无test-standards=停止 | 建议先创建规范 |

## Output

- 测试代码（按项目test source root放置）
- 测试报告：`workspace/{project}/notes/test-{title}.md`

## Route

- 上游：`architecture-design` 确认后进入
- 并行：与 `code-implementation` 由sub-agent并行，共享设计文档
- 下游：全量通过 → 合并主流程
- 异常：基线失败/无规范 → 停止报告

## Self-check

1. 已读 test-standards 和 dev-standards
2. 基线测试改动前运行并记录
3. 每个改动点有对应测试
4. 测试遵循AAA且独立
5. 变更测试有需求追溯
6. 新逻辑覆盖>=80%
7. 测试报告已生成
