---
name: vue-dev-standards
description: Vue.js 编码规范：Composition API、组件设计、Pinia 状态管理。Use when 写 Vue 代码前必读。
---

# Vue.js Development Standards

## 质量红线

| 指标 | 上限 |
|------|------|
| 模板行数 | <= 80 行，超出抽子组件 |
| 组件名 | 必须多词（`UserProfile` 不是 `Profile`） |

## Composition API

- 默认 `<script setup>`，新代码不用 Options API
- 基本类型 → `ref()`，对象 → `reactive()`；禁止解构 `reactive()`（破坏响应式）
- 派生状态 → `computed()`；副作用（API 调用、DOM 操作）→ `watch()`
- 自动依赖追踪 → `watchEffect()`；需要旧值或显式源 → `watch()`
- 解构 props → `toRefs()` 保持响应式
- 可复用逻辑 → composable（`use*` 前缀），一个 composable 做一件事

## 组件

- Props：对象语法 + type + required/default，禁止直接修改 props
- TypeScript：`defineProps<T>()` + `defineEmits<T>()`
- 双向绑定：`defineModel()`，不用 `.sync`
- `v-if` 和 `v-for` 禁止同元素使用
- 列表 `:key` 用稳定唯一 ID，禁止数组 index

## Pinia 状态管理

- 一个 store 一个领域概念，不搞 god store
- 复杂逻辑 → Setup Store（函数语法），简单 CRUD → Option Store
- 禁止在 actions 外直接改 state
- 解构 state/getters → `storeToRefs()`
- 派生数据用 getters，不存冗余

## 禁止项

- Options API（新代码）
- `any` 类型
- 未类型化的 reactive state 和 emits
