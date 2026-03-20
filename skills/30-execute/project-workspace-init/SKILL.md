---
name: project-workspace-init
description: 初始化项目级 workspace 骨架。Use when 首次跟踪一个新项目时，在 OpenSkill-Code/workspace/ 下创建标准目录和默认 markdown 文件。Do not use when 项目 workspace 已存在。
---

# Project Workspace Init

## Purpose

在 `<openskill-code-root>/workspace/{project-name}/` 下创建标准化的项目 workspace，用于持久化项目概览、开发进度、服务器状态和切面笔记。

## Capabilities

- 创建项目级目录骨架
- 生成默认 markdown 状态文件（含安全占位值）
- 为后续的状态记录更新做好准备
- 为项目笔记创建轻量索引文件

## Typical Inputs

- `<openskill-code-root>`：OpenSkill-Code 的本地路径
- `<project-name>`：项目名称（用作目录名，建议 kebab-case）

## Typical Outputs

已初始化的目录树：

```
workspace/{project-name}/
  overview.md              # 项目概览：定位、技术栈、模块、关键路径
  progress.md              # 开发进度：里程碑、当前任务、阻塞项
  servers/
    {server-alias}/
      state.md             # 连接信息、登录方式、运行状态
      login/
        method.md          # 登录方式详情
      services/            # 服务状态（按服务名一个文件）
      timeline.md          # 操作时间线
  notes/                   # 切面笔记：架构决策、排障记录、经验沉淀
    index.md               # 项目笔记索引：主题 -> 文件
```

公共层按需存在：

```
workspace/_global/
  preferences/            # 公共回答偏好、产物偏好
  indexes/
    preferences-index.md  # 公共偏好索引
```

## Standard File Templates

### overview.md

```markdown
# {project-name}

## 定位
（项目一句话描述）

## 技术栈
（主要语言、框架、中间件）

## 模块
| 模块 | 用途 | 端口 |
|------|------|------|

## 关键路径
（核心数据流或业务流程）

## Updated At
YYYY-MM-DDTHH:MM:SSZ
```

### progress.md

```markdown
# Progress

## 当前里程碑
（当前阶段目标）

## 进行中
- [ ] ...

## 已完成
- [x] ...

## 阻塞项
（无）

## Updated At
YYYY-MM-DDTHH:MM:SSZ
```

### servers/{alias}/state.md

```markdown
# Server State: {alias}

## Connection
- Host:
- Port:
- User:

## Login Mode
- Method:
- Identity File:
- SSH Alias:

## Runtime
- Workspace Root:
- Last Verified:
- Status: unknown
```

### servers/{alias}/login/method.md

```markdown
# Login Method

- Method:
- Password Stored: no
- Identity File:
- SSH Alias:
- Updated At:
```

### servers/{alias}/timeline.md

```markdown
# Timeline
```

### notes/index.md

```markdown
# Notes Index

| 主题 | 文件 | 说明 | Updated At |
|------|------|------|------------|
```

## Route

- 首次跟踪新项目时使用此 skill
- 项目 workspace 已存在时跳过，直接用 `project-state-record-update` 更新
- 需要添加服务器时，在已有项目 workspace 的 `servers/` 下新建子目录即可

## Self-check

1. 目录名是否使用了 kebab-case
2. 所有模板文件是否已创建且内容非空
3. 是否避免了在模板中写入猜测性内容
4. 是否创建了 `notes/index.md`
