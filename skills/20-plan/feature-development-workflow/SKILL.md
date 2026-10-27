---
name: feature-development-workflow
description: 需求开发全流程编排：需求分析 → 架构设计 → 并行实现与测试。Use when 收到完整的新功能需求，需要走完从分析到交付的全链路。Do not use when 只是小修改/bug 修复（直接用 L3），或需求还在讨论阶段。
---

# Feature Development Workflow

## Purpose

编排多个 L3 skill 完成从需求到交付的完整开发流程。

## 编排流程

```
Step 1: 需求分析
  └→ L3: requirement-analysis (challenger)
  └→ 产出: workspace/{project}/notes/req-{title}.md
  └→ 用户旅程与业务边界明确后 → Step 2

Step 2: 架构设计
  └→ L3: architecture-design (architect)
  └→ 产出: workspace/{project}/notes/design-{title}.md
  └→ 方案及业务边界明确后 → Step 3

Step 3: 并行实现（sub-agent 分发）
  ┌─ 后端存在改动？
  │  ├→ L3: code-implementation (implementer)     ──┐
  │  └→ L3: test-implementation (tester)           ──┤ 并行
  │                                                  │
  ├─ 前端存在改动？                                    │
  │  ├→ L3: frontend-implementation (fe-impl)      ──┤ 并行
  │  └→ L3: frontend-test-implementation (fe-test) ──┘
  │
  └─ 纯后端/纯前端 → 只启动对应的一组

Step 4: 按影响范围验证（遵守 docs/testing-scope.md，不默认全量）
  └→ 本次相关的后端/前端验证通过；不涉及的一侧不测
  └→ 涉及 SQL 时逐条预执行并评估索引、长期运行和慢事务风险
  └→ 核对业务关键节点日志、[error] 检索及产生日志模块的回收；不适用项说明原因
  └→ 产出: impl-{title}.md + test-{title}.md

Step 5: 独立稳定性复核
  └→ L3: stability-review（子代理查看合并后的最新代码）
  └→ 评估性能、稳定性、现有功能影响；修复后重验并复核最新改动

Step 6: 记录
  └→ L3: project-state-record-update
  └→ 更新 progress.md 与 common/index.md 中已验证的可复用能力
```

## 跳步规则

| 条件 | 可跳过 |
|------|-------|
| 用户已给出明确用户旅程 | Step 1 |
| 用户已给出明确设计方案 | Step 1 + Step 2 |
| 单文件改动，逻辑清晰 | 不走本 L2，直接用 L3 |

## 中断与回退

| 事件 | 处理 |
|------|------|
| Step 1 中需求有歧义 | 继续追问，不跳到 Step 2 |
| Step 2 中发现对代码不了解 | 读取项目文档或 10-understand |
| Step 3 中规范缺失 | 停止，先补充规范 |
| Step 3 相关验证失败 | 定位修复；无关已知失败记录，不扩大阻断 |
| Step 3 中设计与代码不符 | 回退 Step 2 修正方案 |

## Route

- 入口：用户提出完整的新功能需求
- 各 Step 的具体执行由对应 L3 skill 负责
- 本 L2 只做编排和状态衔接，不做具体实现
