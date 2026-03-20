# 编码前必读路由

写代码前根据语言和角色读取对应规范：

## 通用必读

无论什么语言、什么角色，写设计和实现前都先读：

- `docs/architecture-commenting-standards.md`：复杂实现写清设计意图、扩展点、接口预留和结构约束

| 语言 | 角色 | 必读 skill |
|------|------|-----------|
| Java | 写业务代码 | `java-dev-standards` |
| Java | 做技术设计/方案评审 | `java-design-standards` |
| Java | 写测试 | `java-test-standards` |
| Python | 写代码 | `python-dev-standards` |
| Python | 写测试 | `python-test-standards` |
| JS/TS | 写测试 | `javascript-test-standards` |
| Go | 写代码 | `go-dev-standards` |
| Go | 写测试 | `go-test-standards` |
| Rust | 写代码 | `rust-dev-standards` |
| Rust | 写测试 | `rust-test-standards` |
| Kotlin | 写代码 | `kotlin-dev-standards` |
| Node.js | 写后端 | `nodejs-dev-standards` |
| TypeScript/React | 写前端 | `typescript-react-dev-standards` |
| Vue.js | 写前端 | `vue-dev-standards` |
| 前端组件 | 设计/实现 UI 组件 | `frontend-component-design-standards` |
| CSS | 写样式 | `css-styling-standards` |
| HTML | 写标记/交互组件 | `html-accessibility-standards` |
| 数据库 | 设计表结构/写 SQL/DDL 变更 | `database-design-standards` |

## 条件加读

- 涉及 API / 事件 / 信号 / DTO / 配置等**对外协议或 schema** 时，额外读取 `protocol-schema-standards`
