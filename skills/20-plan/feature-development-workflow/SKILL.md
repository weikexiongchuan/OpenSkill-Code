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
  └→ 用户确认用户旅程 + 边界决策 + 测试需求后 → Step 2

Step 2: 架构设计
  └→ L3: architecture-design (architect)
  └→ 产出: workspace/{project}/notes/design-{title}.md
  └→ 用户确认方案 + 边界实现选择 + 测试覆盖边界后 → Step 3

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

Step 4: 全量验证
  └→ 后端测试 + 前端测试全部通过
  └→ 产出: impl-{title}.md + test-{title}.md

Step 5: 记录
  └→ L3: project-state-record-update
  └→ 更新 progress.md
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
| Step 3 中基线测试失败 | 停止，报告用户 |
| Step 3 中设计与代码不符 | 回退 Step 2 修正方案 |

## Route

- 入口：用户提出完整的新功能需求
- 各 Step 的具体执行由对应 L3 skill 负责
- 本 L2 只做编排和状态衔接，不做具体实现
