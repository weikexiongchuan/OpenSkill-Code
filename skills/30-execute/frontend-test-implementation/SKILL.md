---
name: frontend-test-implementation
description: 前端组件测试实现，基于 Testing Library 测试用户行为而非实现细节。Use when 设计确认后需编写前端测试。Do not use when 只做后端测试（用 test-implementation）。
---

# Frontend Test Implementation

## Persona

**用户视角测试工程师** -- 执行期间必须：
- 测试用户看到什么、做了什么，不测内部实现
- 查询元素用 role/label/text，不用 className/testID
- 破坏性思维覆盖边界、异常、无障碍

## 前置条件

- `architecture-design` 完成
- 可与 `frontend-implementation` 并行

## Flow

### Phase 0: 读取规范

1. 读 `typescript-react-dev-standards` 或 `vue-dev-standards`
2. 读 `frontend-component-design-standards`
3. 读 `html-accessibility-standards`
4. 无测试框架（Testing Library/Vitest/Jest）→ 停止，建议先配置

### Phase 1: 基线验证

1. 运行现有测试，记录结果
2. 基线失败 → 停止报告

### Phase 2: 测试用例设计

| 组件层 | 测试重点 | 方法 |
|--------|---------|------|
| 原子 | 渲染变体/Props/无障碍 | Testing Library render + role 查询 |
| 复合 | 交互流程/状态变化 | user-event 模拟 |
| 功能 | 数据加载三态/错误边界/API | MSW mock + findBy 异步 |

最低覆盖：
- 正常渲染: 1 per variant
- 用户交互: 1 per 路径
- 边界/异常: 1 (空数据/错误/loading)
- 无障碍: 1 (axe-core 扫描)

### Phase 3: 实现测试

- **user-event v14+**: `await user.click()` / `await user.type()`，不用 `fireEvent`
- **异步元素**: `findBy*`，不用 `waitFor` + `getBy`
- **API mock**: MSW，不 mock 内部模块
- **无障碍**: 每组件加 `expect(await axe(container)).toHaveNoViolations()`
- **结构**: AAA 模式，一 test 一行为，describe 按组件/变体/状态嵌套

### Phase 4: 禁止事项

| 禁止 | 原因 |
|------|------|
| 快照测试滥用 | 维护负担高，橡皮图章式通过 |
| 测试 CSS | 用视觉回归替代 |
| 测试内部状态 | 测行为不测实现 |
| className/data-testid 查询 | 优先 role/label/text |
| 测第三方库行为 | 不是你的职责 |
| sleep/setTimeout | 用 findBy 或 waitFor |

### Phase 5: 全量验证

1. 既有+新增测试全通过
2. 新组件行为覆盖 100%（按用户行为，非行覆盖）
3. 无障碍扫描零违规
4. 生成报告 `workspace/{project}/notes/test-frontend-{title}.md`

## Output

```markdown
# 前端测试报告: {title}
## 基线结果
## 新增测试
| 组件 | 测试文件 | 覆盖场景 | 状态 |
## 无障碍扫描
| 组件 | 违规数 | 详情 |
## 全量结果
```

## Route

- 上游：`architecture-design`
- 并行：`frontend-implementation`
- 下游：全量通过 → 合并
- 异常：基线失败/无框架 → 停止

## Self-check

1. 只测用户行为，没测实现细节
2. 查询用 role/label/text 优先
3. 用 user-event 而非 fireEvent
4. 每组件跑 axe-core 扫描
5. 避免快照测试滥用
