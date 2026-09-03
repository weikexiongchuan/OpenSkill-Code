# Workspace Protocol

## 目录

- `workspace/_global/preferences/`：跨项目偏好与通用约束。
- `workspace/_global/indexes/preferences-index.md`：公共索引。
- `workspace/{project}/overview.md`：项目概览。
- `workspace/{project}/progress.md`：当前进度。
- `workspace/{project}/servers/`：按服务器隔离的台账。
- `workspace/{project}/notes/`：需求、设计、诊断、实现和规范。
- `workspace/{project}/notes/index.md`：项目笔记索引。

## 记录

1. 先压缩再写，优先短句、表格和引用路径。
2. 状态直接覆盖；需要追溯的历史追加。
3. 同一信息只保存一次，其他位置通过索引引用。
4. 优先记录可复用规则、稳定结论、用户纠偏和项目状态；不记录一次性讨论。
5. 需求、设计和诊断分别用 `req-*`、`design-*`、`bug-*`；组件索引用 `component-registry.md`。
6. 实际推进后更新 `progress.md`；纯讨论或无稳定结论时不写。
7. 文件变大时按主题拆分并同步索引。
