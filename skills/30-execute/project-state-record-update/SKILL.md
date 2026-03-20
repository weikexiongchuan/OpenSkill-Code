---
name: project-state-record-update
description: 更新项目 workspace 中的持久化 markdown 记录。Use when 需要更新 overview、progress、服务器状态、时间线或切面笔记。Do not use when 项目 workspace 尚未初始化（先用 project-workspace-init）。
---

# Project State Record Update

## Purpose

更新一个项目 workspace 中的持久化 markdown 记录。

## Capabilities

- 更新 `overview.md`：项目定位、技术栈、模块变更
- 更新 `progress.md`：里程碑推进、任务状态、阻塞项
- 更新 `servers/{alias}/state.md`：连接信息、运行状态
- 更新 `servers/{alias}/login/method.md`：登录方式变更
- 追加 `servers/{alias}/timeline.md`：操作事件
- 更新 `servers/{alias}/services/*.md`：服务状态
- 创建/更新 `notes/*.md`：架构决策、排障记录、经验沉淀
- 更新 `notes/index.md`：项目笔记索引
- 必要时更新 `workspace/_global/indexes/preferences-index.md`

## Update Rules

1. **只改变化的字段**：不要覆盖整个文件，精确更新变化的部分。
2. **时间线只追加**：`timeline.md` 只追加条目，格式为 `- {ISO8601} {事件描述}`。
3. **新服务器直接建目录**：在 `servers/` 下新建 `{alias}/` 并按 `project-workspace-init` 的模板创建文件。
4. **笔记按主题命名**：`notes/` 下的文件用 kebab-case 命名，如 `redis-migration-decision.md`。
5. **优先沉淀纠偏结果**：用户纠正后的稳定理解、回答偏好、产物约束，更新到可复用笔记，不只留在对话里。
6. **保证后续可命中**：新增笔记时名称要能反映主题，避免后续 skill 无法检索复用。
7. **进度近似必写**：只要任务产生了稳定结论、实际推进或状态变化，通常都应更新 `progress.md`；仅在纯讨论或无有效进展时可跳过。
8. **先压缩再写**：优先把内容压成短条目、短表格；内容变大时按主题拆分，而不是继续堆在原文件。
9. **索引同步更新**：新增或拆分笔记后，必须同步更新对应索引文件。

## Typical Inputs

- `<workspace-root>` 和 `<project-name>`
- 要更新的维度和内容

## Typical Outputs

- 已更新的 markdown 文件

## Route

- 项目 workspace 已存在时使用此 skill
- 项目 workspace 不存在时先用 `project-workspace-init`
- 需要 SSH 连接时先用 `ssh-server-connect`

## Self-check

1. 是否只更新了变化的字段
2. timeline 是否只做了追加
3. Updated At 时间戳是否已更新
