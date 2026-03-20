---
name: html-accessibility-standards
description: HTML 无障碍规范：语义化、ARIA、键盘导航、对比度。Use when 写 HTML 标记或涉及交互组件时必读。
---

# HTML Accessibility Standards

## 语义化

- 优先语义元素：`<nav>`、`<main>`、`<article>`、`<section>`、`<aside>`、`<header>`、`<footer>`
- 操作用 `<button>`，导航用 `<a>`，禁止 `<div onclick>`
- 标题层级有序（h1-h6），不跳级，每页一个 `<h1>`
- 列表用 `<ul>`/`<ol>`，表格数据用 `<table>`

## ARIA

- **第一原则**：原生 HTML 能做的不用 ARIA
- 每个 ARIA role 必须有必需的 states/properties（如 `role="checkbox"` 需要 `aria-checked`）
- 无可见文本的元素 → `aria-label` 或 `aria-labelledby`
- 动态内容更新 → `aria-live="polite"`；错误/紧急 → `"assertive"`
- 可聚焦元素禁止 `role="presentation"` 或 `aria-hidden="true"`

## 键盘与焦点

- 所有交互元素必须键盘可操作（Tab、Enter、Space、Escape、方向键）
- 可见焦点指示器，自定义焦点样式 >= 3:1 对比度
- SPA 路由切换 → 焦点移到主内容或新标题
- 模态框 → 焦点陷阱；关闭后焦点回到触发元素
- tabindex 仅用 `0` 或 `-1`，禁止正值

## 表单

- 每个 `<input>` 必须关联 `<label>`（`for`/`id` 或包裹），placeholder 不是 label
- 相关字段用 `<fieldset>` + `<legend>` 分组
- 校验错误用 `aria-describedby` 指向错误信息元素
- 常见字段加 `autocomplete` 属性

## 视觉

- 正常文本对比度 >= 4.5:1，大文本 >= 3:1（WCAG AA）
- 不仅靠颜色传递信息，辅以文字/图标/纹理
- 支持 `prefers-reduced-motion`：关闭或减弱动画
- 200% 缩放无水平滚动
