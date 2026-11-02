# OpenSkill-Code

Codex／Claude 共用公共入口，由宿主或工作区显式引用；安装、更新和迁移见 [README](README.md)。

## 边界

- 先读适用工作区、项目及模块规则。用户明确修订只替代原适用范围，未知冲突仅澄清受影响部分。
- 只读命中技能及必要引用；小任务直接执行，完整复杂任务才编排。无匹配技能不阻塞；必需规范缺失时报告缺口，不编造要求。
- 按 [最小验证原则](docs/testing-scope.md) 选择验证和复核，允许复用有效结果；未执行不写成通过。
- 不新增未要求的功能、开关或未来扩展机制。复杂实现说明实际原因和边界；异常显式暴露，业务错误语义遵守已确认约定。
- 个人环境和操作偏好留工作区；项目业务、数据、交付和发布规则留项目。按 [约束维护](docs/project-constraint-maintenance.md) 同步入口、模板与技能，不把单个项目选择推广为通用默认。

## 按任务读取

| 任务 | 入口 |
|---|---|
| 开发／设计／测试 | [语言与角色路由](docs/standards-route.md) |
| 完整新功能／复杂修复 | skills/20-plan/ 中匹配的编排技能 |
| 需求／设计未明确 | requirement-analysis／architecture-design；已确认部分不重问 |
| 实现 | code-implementation／frontend-implementation；按需调用测试技能 |
| 文档、报告、表格或规则审核 | [文档交付](docs/document-delivery.md)；方案另读 [设计模板](skills/30-execute/architecture-design/references/design-doc-template.md) |
| 规范沉淀或纠偏 | [约束维护](docs/project-constraint-maintenance.md) → standards-organizer |
| 独立审核、改表／索引方案 | [范围与改表审核](docs/change-scope-schema-review.md)；代码交付按风险进入 stability-review |
| SQL／ORM／批处理 | [SQL 精简与性能验收](docs/sql-performance-validation.md) |
| 日志、错误上报、回收 | [日志规范](docs/logging-standards.md) |
| 复杂实现注释 | [设计意图入码](docs/architecture-commenting-standards.md) |
| 异步任务、队列、状态机 | [生命周期正确性](docs/stateful-workflow-correctness.md) |
| 发布和观察 | 项目部署入口 + [发布验收](docs/release-startup-validation.md)；专项隔离验收仅按明确要求执行 |
| 工具未知 | [工具发现](docs/tool-discovery.md) |
| 记录与复用 | 已有位置 + [workspace protocol](docs/workspace-protocol.md) |
| 技能目录 | [技能索引](docs/skill-index.md) |

技能位于 skills/10-understand、20-plan、30-execute 对应目录；先看 description，再读 SKILL.md 和本次必要 reference。

## 开发检查

1. 读项目规则、对应规范、已有能力及实际代码；缺索引不自动新建记录体系。
2. 核对用户目标、触发条件、错误分类和业务边界，只暂停未明确部分；常规技术细节和验证选择由代理处理。
3. 依赖未验证的外部能力时，先在目标环境跑最小端到端验证；只写必要探针，不提前实现正式模块。已有能力未变可复用证据。
4. 按确认范围选择最小改动；改表先审核方案，长任务检查状态及失败路径，不继承其他场景的限制。
5. 代码开发有远端时 fetch 核对基线；不能同步时说明使用本地代码。
6. 前后端各用相应技能；前端设计覆盖入口、用户旅程、布局、状态、复用、响应式、动效、接口和样式分层。
7. 完成适用 SQL／日志验收和最小验证；小修复自查，复杂或高风险变更独立复核一次，仅补查实质变化。常规发布与专项验收分开。
8. 按需更新已有记录；文档交付前复核。个人约定不提交到业务仓库。

仅首次使用、久未使用或用户明确要求时检查更新，不每轮重复检查。
