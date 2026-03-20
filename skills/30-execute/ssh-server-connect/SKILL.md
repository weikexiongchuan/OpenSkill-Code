---
name: ssh-server-connect
description: 建立 SSH 连通性的原子能力。Use when 需要创建密钥对、安装公钥、密码回退执行、验证 key 登录或启动 SSH 隧道。Do not use when 需要加固 sshd 配置。
---

# SSH Server Connect

## Purpose

提供连接远端服务器所需的最小可复用动作。

## Capabilities

- 确认本地 SSH 密钥存在，不存在则创建
- 在远端安装公钥（需密码访问）
- 密码模式下执行一次性 SSH/SCP 命令
- 后台启动本地 SSH 隧道
- 验证 key 登录

## Typical Inputs

- Host, Port, User
- Identity file 或 public key 路径
- 可选 SSH_PASSWORD

## Typical Outputs

- 本地密钥对
- 远端已安装公钥
- 已验证的 key 登录
- 一次性密码模式命令结果
- 后台隧道 PID 和本地转发端口

## Route

- 任务是"连接服务器"或"让 SSH 可用"时使用此 skill
- 需要更新本地 workspace 状态记录时，切到 `project-state-record-update`
- 需要加固 sshd 时，另行处理（本 skill 不覆盖）

## Load On Demand

- `references/ssh-key-login-workflow.md`
