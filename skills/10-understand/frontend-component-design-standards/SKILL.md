---
name: frontend-component-design-standards
description: 前端组件设计规范：设计 token、组件分层、变体管理、风格一致性。Use when 设计或实现前端 UI 组件前必读。
---

# Frontend Component Design Standards

## 设计 Token 体系

三层 token，禁止魔法值：

| 层 | 示例 | 用途 |
|----|------|------|
| 全局 | `--color-blue-500`, `--spacing-4` | 原始调色板和间距基数 |
| 语义 | `--color-surface`, `--color-text-primary` | 开发者日常接口层 |
| 组件 | `--button-bg`, `--card-padding` | 组件级局部覆盖 |

### 核心 token 规模

- **颜色**：1 套 neutral（50-950）+ 1 primary + 3 语义色（success/warning/error）
- **间距**：4px 基数（4, 8, 12, 16, 20, 24, 32, 40, 48, 64）
- **字号**：5-7 档，每档绑定 line-height + letter-spacing
- **圆角**：2 档（`--radius-sm/md`）
- **阴影**：3 档（`--shadow-sm/md/lg`）
- **z-index**：命名 token（`--z-dropdown: 100; --z-modal: 200`），禁止随意数字

## 组件分层

| 层 | 说明 | 示例 |
|----|------|------|
| 原子 | 不可再拆基础元素 | Button, Input, Badge, Icon |
| 复合 | 多原子组合，有特定语义 | SearchBar, UserCard, DataTable |
| 功能 | 业务级，含数据获取和状态 | SignalDashboard, StrategyEditor |

## 组件设计原则

- **组合优于配置**：用 compound component（`<Card><Card.Header/></Card>`），不搞 20+ props 巨型组件
- **Props <= 7**：相关属性用对象分组，boolean 默认 false，禁止 `isNotX`
- **变体用 TS 联合**：`variant: 'primary' | 'secondary' | 'ghost'`，类型即文档
- **受控/非受控双模式**：提供 `value`/`onChange`（受控）+ `defaultValue`（非受控）
- **转发 ref**：原子组件必须 forwardRef，消费者需 DOM 访问
- **错误边界**：每功能组件包裹 error boundary + 有意义 fallback

## 文件组织

```
Button/
  index.tsx          # 组件实现
  Button.module.css  # 样式
  Button.test.tsx    # 测试
  Button.types.ts    # 类型（复杂时独立）
```

## 风格一致性

- 禁止硬编码颜色/间距/圆角，必须用 token
- Tailwind：锁定 `tailwind.config.ts`，禁用默认调色板，禁止 arbitrary values（`bg-[#xxx]`）
- CSS Modules：启用 `stylelint-declaration-strict-value` 强制 token
- 新组件合并前检查：token 使用/命名约定/TS props/间距圆角一致

## 组件风格索引

项目 workspace 维护 `workspace/{project}/notes/component-registry.md`：

```markdown
# Component Registry
| 组件 | 层 | 变体 | Token 依赖 | 路径 |
|------|---|------|-----------|------|
| Button | 原子 | primary/secondary/ghost | --color-primary, --radius-sm | src/components/Button/ |
```

- 每次新建或修改组件后更新索引
- 实现新组件前先检索，有相似则复用或扩展
- 索引不存在则先扫描现有代码创建

## 页面设计门禁

前端实现前必须有已确认的页面方案，至少说明：

- 页面入口、导航位置和返回路径
- 用户旅程、核心任务和信息优先级
- 桌面端/移动端布局，以及加载、空、失败状态
- 交互、动效、响应式行为和数据接口
- 原子、复合、功能组件的复用关系
- 全局样式、组件样式和页面独有样式的归属

实现后补充实现文档，记录实际组件路径、样式文件、公共层复用和方案偏差。
