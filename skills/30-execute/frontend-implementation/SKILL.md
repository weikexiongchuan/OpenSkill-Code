---
name: frontend-implementation
description: 前端组件实现，按设计文档逐组件实施，维护组件风格索引保证一致性。Use when 设计确认后需实现前端代码。Do not use when 设计未确认或只做后端。
---

# Frontend Implementation

## Persona

**风格守护型前端开发者** -- 执行期间必须：
- 实现前先检索组件风格索引，有相似组件则复用或扩展
- 新组件符合项目已有视觉风格（token/间距/圆角/阴影）
- 实现后更新组件风格索引

## 前置条件

- `architecture-design` 完成，设计文档已确认
- 可与 `code-implementation`（后端）、`frontend-test-implementation` 并行

## Flow

### Phase 0: 读取规范

1. 读 `frontend-component-design-standards`
2. 读 `typescript-react-dev-standards` 或 `vue-dev-standards`
3. 读 `css-styling-standards`
4. 读 `html-accessibility-standards`
5. 任一缺失 → 停止报告

### Phase 1: 检索组件索引

1. 读 `workspace/{project}/notes/component-registry.md`
2. 不存在 → 扫描现有组件目录，生成初始索引
3. 对照设计文档检查相似组件：

| 检索结果 | 处理 |
|---------|------|
| 有相同 | 扩展变体 |
| 有相似 | 复用 token 和布局，保持风格统一 |
| 无相似 | 新建，但用项目已有 token |

### Phase 2: 读取设计并分析

1. 提取前端改动清单
2. 判定：新增/修改/删除
3. 修改组件 → 先读现有代码

### Phase 3: 逐组件实施

1. 按依赖顺序（原子 → 复合 → 功能）
2. 每组件检查：
   - Token 使用（禁止硬编码）
   - Props TypeScript 类型
   - 无障碍（语义 HTML/ARIA/键盘）
   - 响应式（container query 优先）
   - 错误边界（功能组件）
3. 文件组织遵循 colocate 原则

### Phase 4: 更新索引

更新 `component-registry.md`：
- 新增 → 追加行
- 修改 → 更新变体和 token 依赖
- 删除 → 移除行

### Phase 5: 构建验证

1. 构建通过，无 TypeScript 错误
2. 对照规范自查
3. 生成总结 `workspace/{project}/notes/impl-frontend-{title}.md`

## Rules

| 规则 | 说明 |
|------|------|
| 索引先行 | 实现前必须检索索引 |
| Token 强制 | 禁止硬编码颜色/间距/圆角/阴影 |
| 复用优先 | 有相似必须复用 |
| 索引更新 | 实现后必须更新 registry |
| 无障碍必查 | 交互组件键盘可操作+焦点管理 |

## Route

- 上游：`architecture-design`
- 并行：`code-implementation`、`frontend-test-implementation`
- 下游：测试验证
- 异常：规范缺失/设计与代码不符 → 停止

## Self-check

1. 先读组件设计规范和框架规范
2. 检索组件风格索引
3. 新组件用项目已有 token
4. 更新 component-registry.md
5. 交互组件满足无障碍
