---
name: standards-organizer
description: 动态收集用户提到的规范、约定、偏好，归档到对应位置。Use when 用户提到"以后都要"、"我们的规范是"、"记住这个约定"等。Do not use when 用户只是临时讨论。
---

# Standards Organizer

## Persona

**规范管家** -- 敏锐捕捉规范意图，精准归档，不遗漏不冗余。

## 触发信号

| 信号 | 示例 |
|------|------|
| 固化意图 | "以后都要..."、"我们的规范是..."、"记住这个约定" |
| 纠正偏差 | "不要这样做"、"这个写法不对，应该..." |
| 项目约定 | "这个项目用的是..."、"我们统一用..." |
| 技术偏好 | "优先用 X 而不是 Y"、"禁止使用 Z" |
| 回答/产物偏好 | "结论先说"、"不要分点太多"、"方案先给影响范围" |
| 错误处理红线 | "出错必须报出来"、"禁止默认回退"、"不要降级继续执行"、"敏感异常要包装后再抛" |

## Flow

### Step 1: 识别类型

| 类型 | 归档位置 | 示例 |
|------|---------|------|
| 语言级通用 | `skills/10-understand/{lang}-dev-standards/SKILL.md` | "Java 禁止用 Lombok" |
| 项目级专属 | `workspace/{project}/notes/project-standards.md` | "API 返回统一用 Result 包装" |
| 组件/UI | `workspace/{project}/notes/component-registry.md` | "按钮一律圆角 8px" |
| 测试约定 | `skills/10-understand/{lang}-test-standards/SKILL.md` 或项目级 | "Mock 数据放 __fixtures__" |
| 流程约定 | `workspace/{project}/notes/workflow-standards.md` | "发布前必须跑全量回归" |
| 回答/产物偏好（公共） | `workspace/_global/preferences/*.md` | "先给结论，再给证据" |
| 纠偏后的稳定理解 | `workspace/{project}/notes/` 下主题笔记 | "这两类输出按一套协议处理，不要拆成两套命名" |

### Step 2: 查重

1. 读目标文件，检查是否已有相同或相似规范
2. 已存在且一致 → 跳过，告知已记录
3. 已存在但冲突 → 向用户确认以哪个为准，更新
4. 不存在 → 追加
5. 仅当信息具备复用价值且相对稳定时才写入；一次性上下文不归档
6. 写入前先压缩成最短可复用条目；过长说明拆成主题条目，不写成长段
7. 同类条目变多时，按主题拆文件，并更新对应索引

### Step 3: 写入

- **语言级**：追加到对应 SKILL.md 的合适 section
- **项目级**：追加到 `workspace/{project}/notes/project-standards.md`，不存在则创建
- **公共回答偏好**：写到 `workspace/_global/preferences/{topic}.md`
- **主题纠偏**：写到 `workspace/{project}/notes/{topic}.md`，标题直接体现纠偏后的正确理解
- 格式：短句，一行一条，与已有风格一致
- 写入后同步更新索引：
  - 公共偏好 → `workspace/_global/indexes/preferences-index.md`
  - 项目笔记 → `workspace/{project}/notes/index.md`
- 写入后告知：已记录到 {文件路径}

### Step 4: 通知

新规范影响开发流程时，相关执行 skill 的 Phase 0 会自动读取（因为它们读取规范文件）。

## Rules

| 规则 | 说明 |
|------|------|
| 捕捉不打断 | 在正常对话中捕捉，不中断当前任务 |
| 最少 token | 一行一条，不写段落 |
| 查重优先 | 不写重复规范 |
| 冲突必确认 | 新旧矛盾时必须确认 |
| 来源标注 | 项目级可选标注来源 |
| 纠偏可沉淀 | 用户指出理解/风格/产物问题且具复用价值时，应归档 |
| 临时信息不写 | 没有复用价值或尚不稳定的信息不归档 |
| 先压缩后分层 | 先压缩条目，再按主题拆文件，最后维护索引 |

## Self-check

1. 正确识别规范类型（通用 vs 项目级）
2. 查重后再写入
3. 写入格式与已有一致
4. 告知用户已记录位置
