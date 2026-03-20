---
name: javascript-test-standards
description: JavaScript/TypeScript 测试规范：Vitest + Testing Library + MSW + Playwright。Use when 写 JS/TS 测试前必读。
---

# JavaScript / TypeScript Testing Standards

## 推荐栈

| 层 | 工具 | 用途 |
|----|------|------|
| 单元/组件 | **Vitest** | Jest 兼容 API，原生 ESM，5-10x 更快 |
| 组件渲染 | **Testing Library** (React/Vue) | 测用户行为不测实现 |
| API Mock | **MSW 2.x** | 网络层拦截，一套 mock 通吃 |
| E2E | **Playwright** | 自动等待，多浏览器，截图对比 |

## 质量红线

| 指标 | 规则 |
|------|------|
| 用户行为覆盖 | 100%（按行为，非行覆盖） |
| 快照测试 | 禁止滥用，仅用于序列化输出 |
| 实现细节 | 禁止测试 useState/内部方法 |

## 命名与结构

- `describe` 按组件，嵌套 `describe` 按变体/状态
- `it('disables submit when form invalid')` -- 描述行为
- AAA 模式，一个 test 一个行为

## Testing Library 规则

查询优先级（从高到低）：

```
getByRole > getByLabelText > getByText > getByTestId
```

- 用 `user-event v14+`（`await user.click()`），不用 `fireEvent`
- 异步元素用 `findBy*`，不用 `waitFor` + `getBy`
- 状态变更后用 `await waitFor(() => ...)`，不手动 `act()`

## MSW 生命周期

```typescript
beforeAll(() => server.listen())
afterEach(() => server.resetHandlers())
afterAll(() => server.close())
```

- `server.listen()` 放 `beforeAll` 不是 `beforeEach`
- 单测内覆盖 handler：`server.use(http.get(...))`

## Playwright E2E

- 优先 `toHaveScreenshot()` 做视觉回归
- 用 Locator API（`page.getByRole()`），不用 CSS 选择器
- 自动等待已内置，不需要 `page.waitFor*()`

## AI 常见陷阱

| 陷阱 | 正确做法 |
|------|---------|
| 用 `getByTestId` 做首选查询 | 优先 `getByRole` / `getByLabelText` |
| `server.listen()` 放 `beforeEach` | 放 `beforeAll`，`resetHandlers` 放 `afterEach` |
| 忘记 `await` user-event 操作 | `await user.click(button)` |
| 手动 `act()` 包裹 | 用 `waitFor` 或 `findBy` 替代 |
| Jest ESM 配置出错 | 直接用 Vitest，零配置 ESM |
