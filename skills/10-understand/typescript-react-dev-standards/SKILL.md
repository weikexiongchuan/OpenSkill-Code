---
name: typescript-react-dev-standards
description: TypeScript + React 编码规范：组件设计、Hook、类型安全、性能。Use when 写前端代码前必读。
---

# TypeScript / React Development Standards

## 质量红线

| 指标 | 上限 |
|------|------|
| 组件行数 | <= 150 行 |
| JSX 块 | > 50 行或复用 → 抽子组件 |
| Hook 逻辑 | > 10 行或复用 → 抽 custom hook |

## 命名

- 组件：PascalCase，一文件一组件，文件名 = 组件名
- 事件处理：组件内 `handleX`，props 接收 `onX`
- 类型：`interface` 用于对象形状（可扩展），`type` 用于联合/交叉/映射

## 类型安全

- Props 显式定义 interface：`interface ButtonProps { ... }`
- 禁止 `any`，用 `unknown` + 类型收窄
- 禁止非空断言 `!`（除非有注释证明安全），用 `?.` 和 `??`
- 状态变体用判别联合：`{ kind: 'loading' } | { kind: 'error'; message: string } | { kind: 'success'; data: T }`

## Hooks

- 只在顶层调用，不在条件/循环内
- `useEffect` 必须有显式依赖数组，mount-only 的 `[]` 要注释原因
- 有订阅/定时器的 `useEffect` 必须返回 cleanup
- `useMemo` / `useCallback` 只在 profiling 证明需要时用，不预优化

## 数据获取

- `useEffect` 内 HTTP 请求必须用 `AbortController`
- 显式处理 loading / error / success 三态
- 列表必须有稳定唯一 `key`，禁止用数组 index（可重排场景）

## 状态管理

- UI 状态 → `useState`
- 先提升状态，再考虑全局方案
- Context 用于低频数据（theme、auth），高频数据用专门状态库

## 性能

- JSX props 禁止内联对象/数组字面量（导致重渲染），提取为常量或 `useMemo`
- 避免在渲染路径做昂贵计算

## 样式

- CSS Modules 或 Tailwind，动态值除外禁止内联 style
- 类名用 BEM 命名或 utility-first

## 禁止项

- `any` 类型
- 生产代码 `console.log`
- 表情符号
