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

先按 [最小验证原则](../../../docs/testing-scope.md) 判断是否需要测试；小需求、小调整可仅做差异自查或目视检查，不进入后续测试流程。需要时只验证相关交互，不默认全组件、全站测试。

## 前置条件

- `architecture-design` 完成
- 交互业务语义已明确；代理选择相应测试，不重复询问测试策略
- 可与 `frontend-implementation` 并行

## Flow

### Phase 0: 读取规范

1. 读 `typescript-react-dev-standards` 或 `vue-dev-standards`
2. 读 `frontend-component-design-standards`
3. 读 `html-accessibility-standards`
4. 无测试框架时用最小可行验证，不为小修引入框架

### Phase 1: 基线验证

1. 按需运行受影响组件测试，已有有效结果可复用
2. 相关失败定位；无关既有失败记录

### Phase 2: 测试用例设计

先核对实际改变的交互语义，只有业务结果不明确才询问；常规测试场景由代理选择。

| 组件层 | 测试重点 | 方法 |
|--------|---------|------|
| 原子 | 渲染变体/Props/无障碍 | Testing Library render + role 查询 |
| 复合 | 交互流程/状态变化 | user-event 模拟 |
| 功能 | 数据加载三态/错误边界/API | MSW mock + findBy 异步 |

仅选择本次相关的覆盖项，不要求全部执行：
- 正常渲染: 1 per variant
- 用户交互: 1 per 路径
- 边界/异常: 1 (空数据/错误/loading)
- 无障碍: 1 (axe-core 扫描)

### Phase 3: 实现测试

- **user-event v14+**: `await user.click()` / `await user.type()`，不用 `fireEvent`
- **异步元素**: `findBy*`，不用 `waitFor` + `getBy`
- **API mock**: MSW，不 mock 内部模块
- **无障碍**: 涉及相关语义/交互变化时加 `expect(await axe(container)).toHaveNoViolations()`
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

### Phase 5: 相关验证

1. 本次相关测试或交互检查通过
2. 关键变化已覆盖，不设统一100%门槛
3. 涉及无障碍变化才执行对应扫描
4. 小修复简述结果；复杂变更按需记录报告

## Output

```markdown
# 前端测试报告: {title}
## 基线结果
## 新增测试
| 组件 | 测试文件 | 覆盖场景 | 状态 |
## 无障碍扫描
| 组件 | 违规数 | 详情 |
## 本次验证结果
```

## Route

- 上游：`architecture-design`
- 并行：`frontend-implementation`
- 下游：相关验证通过 → 合并
- 异常：相关失败定位处理；无框架不自动阻断
- 异常：业务语义未明确 → 只澄清受影响部分

## Self-check

1. 只测用户行为，没测实现细节
2. 查询用 role/label/text 优先
3. 用 user-event 而非 fireEvent
4. 按本次无障碍影响选择扫描
5. 避免快照测试滥用
