# OpenSkill-Code

AI 辅助开发技能集入口。

协助 AI 读取现有项目、遵循项目规则，并在实际工作中逐步沉淀和维护项目约束。通用层提供识别、核实与归档的方法；具体平台、业务选择和操作要求保留在项目中，不因某个项目采用就推广为通用要求。

前提：已注册到用户级 `~/.claude/CLAUDE.md`；项目级 `CLAUDE.md` 只补充项目约束。

## 激活条件

| 场景 | 动作 |
|------|------|
| 收到代码开发需求（新功能/bug 修复/重构/优化） | 走**开发前确认清单** |
| 用户明确约定、稳定纠偏，或任务中需要沉淀项目约束 | 按 `docs/project-constraint-maintenance.md` 区分规则、事实与候选约束，再由 `standards-organizer` 归档 |
| 需要理解不熟悉的项目/架构/模块 | 先按 `docs/project-constraint-maintenance.md` 读取适用项目规则与相关证据，再找匹配 skill；纯阅读不自动创建规范文件 |
| 需要多个 skill 协作完成的复杂任务 | 扫描 `skills/20-plan/` 找编排 skill |
| 用户要求改进 skill 或反馈问题 | 检查对应 skill 与入口；稳定规范用 `standards-organizer` 归档 |

## 验证规模

所有测试与交付条款先按 [最小验证原则](docs/testing-scope.md) 判断范围。小需求、小调整和小修复由代理自行评估是否需要测试，低风险时可仅做差异自查；不重复测试确认、不强制多层测试或多轮审核；无相关代码变化时复用已有结果。

## 开发前确认清单

收到开发需求后，按顺序确认：

```
1. [规范] 对应语言的开发规范是否已读取？
   └→ 查 docs/standards-route.md，读取对应 *-dev-standards
   └→ 同时读取 docs/architecture-commenting-standards.md，统一执行“设计意图入码”规范
   └→ 规范不存在 → 停止，先用 standards-organizer 补充

2. [项目规范] 该项目是否有专属规范？
   └→ 先读适用的项目/模块 AGENTS.md、CLAUDE.md 及其引用；按 docs/project-constraint-maintenance.md 核实任务相关证据，不将代码现状直接升级为约束
   └→ 读取项目已有规范和可复用能力索引及命中条目；采用 OpenSkill workspace 时读取其 notes/project-standards.md 与 common/index.md
   └→ 缺少索引时仅在本次确需记录时沿用现有目录补建，不自动初始化另一套 workspace

3. [需求] 需求是否已明确？
   └→ 未明确 → requirement-analysis（challenger 人格追问）
   └→ 已明确 → 继续

4. [调研可行性] 后续实现是否依赖尚未验证的外部能力？
   └→ 涉及第三方接口、App、设备、浏览器、代理或特定服务器环境 → 必须先在最终目标环境跑通最小端到端链路
   └→ 验证阶段只允许编写最小验证脚本或配置，禁止提前实现正式模块、接口、数据库、队列、调度和部署代码
   └→ 验证失败或核心数据不符合成功判据 → 标记“验证阻塞”，记录证据并停止后续实现
   └→ 验证通过 → 记录环境、命令、输入、真实响应、解析结果和核心数据证据，再继续设计

5. [业务边界] 尚有会改变业务结果的未明确边界吗？
   └→ 必须先由 challenger 人格定义边界清单：正常、异常、空值、重复、并发、时序、窗口清理、错误暴露、协议兼容
   └→ 对每个存在多种实现方式的边界，必须让用户选择，不得 AI 自行决定
   └→ 代理按改动风险选择最小验证；已确认业务边界不重复询问测试策略
   └→ 未确认 → requirement-analysis 继续追问并沉淀

6. [设计] 实现方案是否已确认？
   └→ 未确认 → architecture-design（architect 人格设计）
   └→ 逐项核对“用户需求 → 设计步骤 → 实际改动”；任何设计步骤都必须直接对应用户明确要求，无法对应的删除，存在多种实现方式的先向用户确认
   └→ 扩展点、可替换接口、未来兼容约束及设计意图注释，仅在用户明确要求时加入
   └→ 业务边界只记录用户已明确要求；常规验证范围按风险选择并简述
   └→ 长任务按 docs/architecture-commenting-standards.md 检查逐阶段失败处理与测试；方案文档的输出结构统一遵守 skills/30-execute/architecture-design/references/design-doc-template.md
   └→ 前端方案必须包含：页面入口、用户旅程、布局、状态、组件复用、响应式、动效、数据接口和样式分层
   └→ 涉及 SQL 时按 docs/sql-performance-validation.md 逐条复检能否精简并采用更高效的等价路径，再设计索引、执行计划和长事务边界；包含 ORM/工具实际生成 SQL
   └→ 表结构/索引变更在正式实现前按 docs/change-scope-schema-review.md 交独立子代理审核必要性、免改表替代方案、真实规模与执行影响；范围扩大或新增风险先确认具体方案
   └→ 按 docs/logging-standards.md 设计关键节点、稳定标签、错误标签和日志回收
   └→ 已确认 → 继续

7. [代码同步] 是否基于最新代码？
   └→ 有远端 → git fetch 确认
   └→ 无远端 → 标注使用本地代码

8. [执行] 按已明确范围实现，需要测试时再启用对应测试 skill：
   └→ 后端：code-implementation；按需 test-implementation
   └→ 前端：frontend-implementation；按需 frontend-test-implementation
   └→ 纯后端任务不触发前端 skill，反之亦然
   └→ 实现复杂逻辑时，必须把“为什么这样设计”“后续如何扩展”“哪些接口是预留点”写在临近代码的注释里

9. [交付前] 最新代码是否完成验证与独立复核？
   └→ 发包/部署按 docs/release-startup-validation.md 覆盖目标实际启用组合，验证真实容器装配及最终制品启动；未通过不得覆盖线上制品或停止旧服务
   └→ 涉及 SQL 时复检最终 SQL、参数及执行方式是否仍为最简高效等价路径，完成实际预执行、适用语句的执行计划及长期运行/慢事务评估；DDL 改做隔离库迁移、锁和恢复验证，生产写入另需该次明确授权
   └→ 验证业务日志关键节点、[error] 检索和产生日志模块的回收机制；不适用项说明原因
   └→ 小修复自查范围及相关验证即可；涉及改表、广泛公共行为或高风险并发时，再按 docs/change-scope-schema-review.md 独立复核；修复只复核受影响部分，不重启整套流程
   └→ 更新项目 common/index.md 与可复用条目，记录真实复用边界和代码位置
```

小任务（明确单文件修改、bug 修复）采用最小验证原则；已有外部能力未变时复用已有证据，不重做端到端调研。

## 使用规则

1. **确认清单前置**：开发需求必须走开发前确认清单，不跳步
2. **规范前置**：写代码前必须先读对应规范，不读不写。规范不存在则先补充
3. **设计意图入码**：复杂实现、关键权衡、早期预留接口、结构性约束，必须落在代码注释或接口文档中，不能只留在对话里
4. **边界由用户拍板**：边界条件、异常语义、窗口清理、重复触发、空值处理、协议行为等，只要存在多种合理实现，必须先由 challenger 人格列出并让用户选择，不能 AI 自己定
5. **测试按风险选取**：需求明确时直接选择最相关验证，不逐阶段询问；只澄清尚未确定的业务语义或新增权限
6. **最小充分验证**：小需求、小调整和小修复可按风险决定不跑测试，简述依据即可；需要时只做相关验证，不因无关提交或上传重试重跑已通过测试
7. **错误必须显式暴露**：代码出错必须报错并让用户可见；禁止默认回退、默认值兜底、静默跳过、自动降级继续执行，无论启动阶段还是运行阶段都一样
   - 启动出错：必须中断停止，直接 fail fast
   - 运行出错：必须抛出异常，并打印 `error` 日志，包含堆栈和必要上下文
   - 异常暴露：必须遵守安全规范；底层敏感异常先记录日志，再包装成安全异常对外抛出
8. **匹配优先**：先看 skill 的 `description` 判断是否匹配当前任务
9. **无匹配则降级**：没有匹配的 skill 时，按常规方式处理，不阻塞
10. **一个 skill 只做一件事**：不要把多个 skill 混合执行
11. **L3 优先**：单个 skill 能完成的放 L3；只有需要编排多个 skill 协作时才用 L2
12. **调研先于实现**：调研结论决定后续实现时，必须先在最终目标环境证明核心链路可运行。本地成功、代码推断、接口文档或框架编译通过，都不能代替端到端验证
13. **验证失败即停止**：安装、启动、认证、访问、响应解析或核心数据任一环节未通过时，不得继续正式开发或部署；禁止用“先搭框架、后补验证”推进
14. **前端文档门禁**：前端必须先有已确认的方案文档，完成后补实现文档；没有页面位置、用户旅程、布局、交互、组件复用、动效和样式分层时不得实现
15. **样式分层**：全局 token、reset、基础布局和通用动效放公共层；页面和业务组件独有样式就近放置，禁止持续堆入单个全局 CSS 文件
16. **CSS 文件上限**：新增或修改的 CSS 文件原则上不超过 500 行；超过必须按 token/base/layout/component/page 拆分，并记录拆分关系
17. **上下文节制**：只读取当前任务需要的索引、规范和模块；用引用路径代替重复粘贴，不重复记录同一规则、代码、日志和背景
18. **禁止多余开关**：开发功能时禁止新增用户未明确要求的配置开关、功能开关或模式开关；确有多种运行方式时，必须先向用户确认
19. **SQL 验收**：新增/修改 SQL 必须最大化利用索引，按 [SQL 性能验收](docs/sql-performance-validation.md) 预执行并评估长期运行风险；不得把未执行写成已验证
20. **日志可检索且可回收**：每个产生日志的模块必须有轮转/保留机制，业务关键节点使用稳定标签，错误日志统一带 `[error]`；按 [日志规范](docs/logging-standards.md) 验收
21. **复核按风险执行**：小修复不强制子代理；复杂或高风险变更按 `stability-review` 独立复核一次，只对新增实质风险补查

## 上下文与项目结构

- 入口文件只保留路由、门禁和不可破坏约束；详细规则放对应 skill，长 schema/流程放 `references/`。
- 一个规则只维护一份；其他位置只写索引或引用路径，禁止复制整段规范、日志和代码。
- 读取顺序固定为：命中 skill 的 `description` → `SKILL.md` → 被引用的单个 reference；不要预加载整个目录。
- 方案文档结构遵守 [设计文档规范](skills/30-execute/architecture-design/references/design-doc-template.md)；实现文档只记录实际改动、验证结果和偏差。
- 页面、组件和样式按职责拆目录；不要把多个业务页面、组件和全局样式合并成单个超大文件。

## Workspace 约定

- 优先沿用项目已有记录位置；选择 OpenSkill workspace 时，公共记忆为 `workspace/_global/`，项目记忆为 `workspace/{project}/`，不复制仓库已有规则。
- 在所选记录位置按需读取已有公共索引、`progress.md`、项目 `common/index.md` 与 `notes/index.md` 及命中条目；不存在的文件不因读取而自动创建。
- 写入前先压缩；状态覆盖、历史追加、同一信息只保存一次，并维护索引。
- 详细目录和归档规则见 [workspace protocol](docs/workspace-protocol.md)。

## 人格编码

| 编码 | 含义 | 生效场景 |
|------|------|---------|
| `challenger` | 反驳型：不假设、不脑补、逐点追问 | `requirement-analysis` |
| `architect` | 严谨型：决策必须有代码依据，改动必须评估影响 | `architecture-design` |
| `implementer` | 精确执行型：严格按设计文档，不擅自扩大范围 | `code-implementation` |
| `frontend-implementer` | 风格守护型：检索组件索引，token 强制，实现后更新索引 | `frontend-implementation` |
| `tester` | 破坏性思维：寻找边界和异常，基线失败立即停止 | `test-implementation` |
| `frontend-tester` | 用户视角：测行为不测实现，axe-core 无障碍扫描 | `frontend-test-implementation` |
| `diagnostician` | 侦探型：先复现后推断，假设必须可验证，修根因不修表象 | `bug-diagnosis` |

## 文档索引

| 何时读取 | 文档 |
|---------|------|
| 选择测试、回归及发布验证范围 | [docs/testing-scope.md](docs/testing-scope.md) |
| 写代码前查对应规范 | [docs/standards-route.md](docs/standards-route.md) |
| 理解现有项目、沉淀或维护项目约束 | [docs/project-constraint-maintenance.md](docs/project-constraint-maintenance.md) |
| 设计或验收 SQL | [docs/sql-performance-validation.md](docs/sql-performance-validation.md) |
| 子代理审核范围、设计或评审改表方案 | [docs/change-scope-schema-review.md](docs/change-scope-schema-review.md) |
| 设计或验收模块日志 | [docs/logging-standards.md](docs/logging-standards.md) |
| 写设计/实现时统一执行设计意图入码规范 | [docs/architecture-commenting-standards.md](docs/architecture-commenting-standards.md) |
| 设计或诊断异步任务、队列、调度和状态机 | [docs/stateful-workflow-correctness.md](docs/stateful-workflow-correctness.md) |
| 发包、部署或审核启动与配置装配 | [docs/release-startup-validation.md](docs/release-startup-validation.md) |
| 查找可用 skill | [docs/skill-index.md](docs/skill-index.md) |
| 首次使用构建工具 | [docs/tool-discovery.md](docs/tool-discovery.md) |

## 更新检查

- 仅在首次使用、久未使用后再次使用、或用户明确要求时低频检查更新；不要每次激活、每次任务、每轮对话都检查
