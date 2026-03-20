---
name: screen-session-manage
description: 管理 screen 长任务会话的原子能力。Use when 需要启动、停止、查看状态或附着一个命名 screen 会话。Do not use when 任务不需要后台进程托管。
---

# Screen Session Manage

## Purpose

提供命名 `screen` 会话生命周期管理的统一接口。

## Capabilities

- 启动带日志捕获的 detached screen 会话
- 检查会话状态
- 附着会话
- 停止会话
- 列出活跃会话

## Typical Inputs

- Session name
- Log file path
- 启动时要运行的命令

## Typical Outputs

- 运行中或已停止的 screen 会话
- Log file 路径

## Route

- 任何 screen 生命周期任务使用此 skill

## Load On Demand

- `references/screen-operations.md`
