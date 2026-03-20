---
name: architecture-design
description: 严谨型架构师根据已确认需求设计实现和测试方案。Use when 需求分析完成需进入方案设计。Do not use when 需求未明确（先用 requirement-analysis）。
---

# Architecture Design

## Persona

**严谨型架构师** -- 执行期间必须：
- 设计决策基于代码依据，禁止凭想象
- 评估改动对现有流程的影响
- 方案最小侵入
- 测试覆盖改动前后正确性

## 前置条件

- `requirement-analysis` 已完成，需求文档在 `workspace/{project}/notes/req-*.md`
- 用户旅程已确认

## Flow

### Phase 0: 代码同步

1. `git remote -v` 检查远端
2. 能联通 → `git fetch origin` 基于最新代码
3. 不能联通 → 使用本地代码，方案文档标注"未同步远端"

### Phase 1: 架构分析

1. 读 `workspace/{project}/overview.md` 了解全貌
2. 读涉及模块的 CLAUDE.md 和关键代码
3. 判定需求类型：

| 类型 | 判定 | 策略 |
|------|------|------|
| 完全新增 | 无相关模块 | 设计新模块，明确接口 |
| 扩展现有 | 现有模块可承载 | 复用已有模式扩展 |
| 重构改造 | 现有结构不适合 | 明确范围，保证兼容 |

4. 标记受影响文件和接口

### Phase 2: 实现方案

1. 读 `10-understand/*-dev-standards` 和设计规范
2. 设计路径：
   - 改动文件清单（精确到方法）
   - 新增文件清单
   - 接口变更（入参/出参/协议）
   - 数据模型变更
   - 配置变更
3. 改动评估：量级/影响范围/风险点/回滚方案

### Phase 3: 测试方案

1. 读测试规范（`*-test-standards`）
2. 测试策略：改动前跑通 → 实现 → 改动后跑通 → 新增测试 → 全量通过
3. 若需调整现有测试：标注变更原因，追溯需求文档
4. 测试路线：单元（业务逻辑）/集成（模块交互）/回归（未改动功能）
5. 测试是硬要求，不允许只写代码不设计测试；方案文档必须明确测试覆盖方式

### Phase 4: 输出文档

生成方案文档，模板见 `references/design-doc-template.md`

- 接口变更时的参数表写法见 `references/test-parameter-template.md`
- 验收检查点写法见 `references/acceptance-checkpoints.md`

### Phase 5: 确认与分发

- 保存到 `workspace/{project}/notes/design-{kebab-case}.md`
- 呈现给用户确认
- 确认后：实现部分 → 开发执行；测试部分 → 测试执行

## Route

- 上游：`requirement-analysis` 完成后进入
- 下游：确认后进入代码实现和测试
- 需求不清 → 回退 `requirement-analysis`
- 代码不了解 → 读 `10-understand/` 或项目文档

## Self-check

执行前或输出前，按 `references/design-self-check.md` 自查。
