---
name: css-styling-standards
description: CSS/样式规范：BEM、布局、性能、特异性管理。Use when 写样式代码前必读。
---

# CSS / Styling Standards

## 特异性

- 组件样式最多一个 class 选择器，禁止 ID 选择器
- 层级：reset → base → layout → components → utilities
- `!important` 仅限 utility class
- 嵌套选择器最多 3 层，超出重构为扁平 class

## BEM 命名

- Block = 独立组件（`.card`）
- Element = 子元素（`.card__title`）
- Modifier = 变体（`.card--featured`）
- 禁止 `.block__element__sub-element`（不超过两级）
- 一次性覆盖用 utility class（`.u-visually-hidden`）

## 布局

- 一维 → Flexbox，二维 → Grid，禁止 float 布局
- 间距用 `gap`，不用 margin hack
- 流式尺寸用 `min()` / `max()` / `clamp()`
- 全局 `box-sizing: border-box`

## 性能

- 只动画合成器属性：`transform`、`opacity`、`filter`
- 禁止动画 `width`/`height`/`top`/`left`
- `will-change` 仅动画前加、动画后移除
- 禁止 CSS `@import`，用构建工具打包

## 主题

- 用 CSS 自定义属性（`--color-primary`）做运行时主题
- Sass 变量仅用于编译期常量

## 禁止项

- 内联 style（动态计算值除外）
- `outline: none` 不提供替代焦点指示器
- 深嵌套选择器（>3 层）
