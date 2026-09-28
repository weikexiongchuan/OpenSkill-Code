# Workspace Protocol

## 目录

- `workspace/_global/preferences/`：跨项目偏好与通用约束。
- `workspace/_global/indexes/preferences-index.md`：公共索引。
- `workspace/{project}/overview.md`：项目概览。
- `workspace/{project}/progress.md`：当前进度。
- `workspace/{project}/common/`：项目可复用能力层（已验证的公共代码、接口、查询、运行约束），不是复制源码。
- `workspace/{project}/common/index.md`：可复用能力索引；按需链接同目录主题笔记及真实代码位置。
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
8. 需求和设计阶段先读项目 `common/index.md` 与命中的主题笔记，核对已有能力、适用边界和真实代码；不能把过期笔记当成事实。
9. 开发过程中发现可复用能力或复用边界变化时及时更新 `common/`；条目记录用途、代码位置、适用/不适用条件和最近验证日期。无法证实的内容不得写成已验证能力。
10. `common/` 只存项目内可复用能力；跨项目通用约束进 `_global/` 或通用 skill，单次需求/故障细节留在 `notes/`。同一事实仅保留一处，其余用链接引用。
