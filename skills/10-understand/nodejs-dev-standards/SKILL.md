---
name: nodejs-dev-standards
description: Node.js/Express 后端规范：异步、事件循环、内存泄漏、安全。Use when 写 Node.js 后端代码前必读。
---

# Node.js / Express Development Standards

## 质量红线

| 指标 | 规则 |
|------|------|
| 同步IO | 请求处理中禁止 `fs.readFileSync` 等同步 IO |
| 日志 | 生产禁止 `console.log`，用结构化 logger（Pino/Winston） |
| 输入验证 | 边界处必须验证用户输入（Zod/Joi），reject early |
| 密钥 | 禁止硬编码，启动时从 env 加载并用 schema 校验 |
| Promise | 所有 Promise rejection 必须处理（Node 15+ 未处理会 crash） |

## 异步与错误处理

- async 路由处理器必须 catch：`const asyncHandler = fn => (req, res, next) => Promise.resolve(fn(req, res, next)).catch(next)`
- 集中错误处理在单个 Express error middleware `(err, req, res, next)`
- 区分操作错误（bad input、timeout → 优雅响应）和编程错误（TypeError → crash + 重启）
- 外部请求用 `AbortController` 设超时

## 事件循环保护

- CPU 密集 → `worker_threads` 或任务队列（BullMQ）
- 大文件用 stream（`fs.createReadStream` + `pipeline()`），不缓冲到内存
- 避免 ReDoS：用户输入的正则用 `safe-regex` 或 `re2`

## 内存泄漏防范

- 单次触发用 `.once()` 优于 `.on()`
- 关机时清理 `setInterval`/`setTimeout`
- DB 连接、stream、socket 在 `finally` 或 shutdown hook 中关闭
- 避免闭包捕获大对象在长生命周期回调中

## 项目结构

- `routes/` → `controllers/` → `services/` → `models/`，业务逻辑在 services 不在 handler
- DB 用连接池（pg-pool / mongoose pool），禁止每请求新建连接
- env 启动时 schema 校验，配置错误 fail fast
- 新项目用 ESM（`"type": "module"`），非 trivial 项目用 TypeScript
