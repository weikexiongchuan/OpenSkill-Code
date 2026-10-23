---
name: project-state-record-update
description: 维护项目已有的持久化记录，包括进度、事实、纠偏、服务器状态和索引。Use when 实际工作需要更新这些记录；优先沿用项目位置，仅在选用 OpenSkill workspace 时按需初始化。
---

# Project State Record Update

## Purpose

更新项目实际采用的持久化 markdown 记录。优先沿用仓库已有文件和索引；以下固定目录、文件名和补建模板仅在选用 OpenSkill workspace 时适用，已有项目按现有记录分工维护，不强制补建 progress.md 或 common/index.md。

## Capabilities

- 更新 `overview.md`：项目定位、技术栈、模块变更
- 更新 `progress.md`：里程碑推进、任务状态、阻塞项
- 更新 `servers/{alias}/state.md`：连接信息、运行状态
- 更新 `servers/{alias}/login/method.md`：登录方式变更
- 追加 `servers/{alias}/timeline.md`：操作事件
- 更新 `servers/{alias}/services/*.md`：服务状态
- 创建/更新 `notes/*.md`：架构决策、排障记录、经验沉淀
- 更新 `notes/index.md`：项目笔记索引
- 创建/更新 `common/index.md` 与主题笔记：项目内已验证的可复用能力及边界
- 必要时更新 `workspace/_global/indexes/preferences-index.md`

## Update Rules

1. **只改变化的字段**：不要覆盖整个文件，精确更新变化的部分。
2. **时间线只追加**：`timeline.md` 只追加条目，格式为 `- {ISO8601} {事件描述}`。
3. **新服务器直接建目录**：在 `servers/` 下新建 `{alias}/` 并按 `project-workspace-init` 的模板创建文件。
4. **笔记按主题命名**：`notes/` 下的文件用 kebab-case 命名，如 `redis-migration-decision.md`。
5. **优先沉淀纠偏结果**：按 [项目约束的识别与维护](../../../docs/project-constraint-maintenance.md) 区分规则、事实和候选；优先更新项目已有维护位置，workspace 仅引用已有规则。用户纠正后的稳定理解和约束保留来源与范围，不将本次参数或代码现状写成长期要求。
6. **保证后续可命中**：新增笔记时名称要能反映主题，避免后续 skill 无法检索复用。
7. **进度近似必写**：只要任务产生了稳定结论、实际推进或状态变化，通常都应更新 `progress.md`；仅在纯讨论或无有效进展时可跳过。
8. **先压缩再写**：优先把内容压成短条目、短表格；内容变大时按主题拆分，而不是继续堆在原文件。
9. **索引同步更新**：新增或拆分笔记后，必须同步更新对应索引文件。
10. **公共层持续维护**：需求、设计、实现中发现已验证的项目内公共能力时，更新 `common/index.md`；记录真实代码路径、适用/不适用边界和最近验证日期。已有项目缺少该文件时按 `project-workspace-init` 模板补建；失效条目及时修订或删除，不复制源码、临时日志或未核实结论。

## Typical Inputs

- `<workspace-root>` 和 `<project-name>`
- 要更新的维度和内容

## Typical Outputs

- 已更新的 markdown 文件

## Route

- 项目已有记录位置时直接更新相应文件，不自动初始化 OpenSkill workspace
- 本次需要持久化且明确选用 OpenSkill workspace 时，已存在则更新，尚未初始化才用 `project-workspace-init`
- 需要 SSH 连接时先用 `ssh-server-connect`

## Self-check

1. 是否只更新了变化的字段
2. timeline 是否只做了追加
3. Updated At 时间戳是否已更新
4. 可复用能力变化是否已同步 `common/index.md`，且索引指向真实代码
