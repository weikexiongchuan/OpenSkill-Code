---
name: bug-fix-workflow
description: Bug 修复全流程编排：诊断 → 修复设计 → 实现 + 测试。Use when 需要走完从排查到修复的完整流程。Do not use when bug 原因已明确只需单文件修改（直接用 L3）。
---

# Bug Fix Workflow

## Purpose

编排多个 L3 skill 完成从 bug 诊断到修复验证的完整流程。

## 编排流程

```
Step 1: 诊断
  └→ L3: bug-diagnosis (diagnostician)
  └→ 产出: workspace/{project}/notes/bug-{id}.md
  └→ 根因定位后 → Step 2

Step 2: 修复设计
  └→ L3: architecture-design (architect, 轻量模式)
  └→ 输入: bug 诊断报告中的根因 + 影响范围
  └→ 产出: 修复方案追加到 bug-{id}.md
  └→ 用户确认方案后 → Step 3

Step 3: 实现修复 + 回归测试（并行）
  ┌─ L3: code-implementation (implementer) ──┐
  │  └→ 按修复方案改动                        │ 并行
  └─ L3: test-implementation (tester)       ──┘
     └→ 基线测试 → 修复验证测试 → 全量回归

Step 4: 验证
  └→ 复现路径再走一遍，确认 bug 已修复
  └→ 全量测试通过

Step 5: 记录
  └→ L3: project-state-record-update
  └→ 更新 progress.md
```

## 跳步规则

| 条件 | 可跳过 |
|------|-------|
| 用户已给出明确根因和修复方案 | Step 1 + Step 2 |
| 根因明确，修复方案显而易见（单行改动） | 不走本 L2，直接用 L3 |
| 纯前端 bug | Step 3 替换为 frontend-implementation + frontend-test-implementation |

## 中断与回退

| 事件 | 处理 |
|------|------|
| Step 1 无法复现 | 追问上下文，不跳到 Step 2 |
| Step 2 发现影响范围大于预期 | 升级为 feature-development-workflow |
| Step 3 基线测试失败 | 停止，报告用户 |
| Step 4 复现路径仍能触发 bug | 回退 Step 1 重新诊断 |

## Route

- 入口：用户报告 bug 或异常行为
- 各 Step 的具体执行由对应 L3 skill 负责
- 本 L2 只做编排和状态衔接，不做具体实现
