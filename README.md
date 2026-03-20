# OpenSkill-Code

AI 辅助开发技能集 -- 本文件是安装引导，AI 只在首次读取时执行，之后不再读取。

## Install Protocol

当 AI 首次读取到本文件时，执行以下安装流程：

### Step 1: 确认存放位置

- 用户指定路径 → 使用指定路径
- 用户未指定 → 默认 `~/.claude/OpenSkill-Code/`
- 项目内路径、workspace 路径、用户自定义路径都合法，**始终以用户确认路径为准**
- 当前位置即最终位置 → 跳到 Step 2

### Step 1.5: 确认当前项目根目录

- “当前项目”指用户当前正在操作、并需要接入 OpenSkill-Code 的项目根目录
- 优先使用用户显式给出的项目路径
- 若用户未显式给出，则以当前仓库根目录为准
- 如果项目根目录不明确，必须先确认，再执行写入
- **禁止**在项目根目录不明确时向多个 `CLAUDE.md` 盲写

### Step 2: 注册到用户级 CLAUDE.md

先在用户级 `~/.claude/CLAUDE.md` 中写入或更新 OpenSkill-Code 索引段。  
目标：**无论从哪个目录打开，AI 都能先识别到 OpenSkill-Code 入口。**

写入内容至少包括：

```markdown
## OpenSkill-Code

- **本地路径**: `<CONFIRMED_PATH>`
- **入口**: 读取 `<PATH>/CLAUDE.md`
- **按需读取**: 仅在命中时读取对应 SKILL.md / references
- **更新**: 仅首次使用、久未使用后再次使用、或用户明确要求时低频检查
```

写入规则：

- 如果用户级 `~/.claude/CLAUDE.md` 中已存在 `OpenSkill-Code` 段，只更新路径和说明，**禁止重复追加**
- 如果用户级 `~/.claude/CLAUDE.md` 不存在，则先创建，再写入索引段
- 如果无法创建或无法写入，必须显式报错，不得跳过或假装注册成功

### Step 3: 写入项目级 CLAUDE.md

在**当前项目**的 CLAUDE.md（项目根目录）中追加以下索引段（若已存在则更新路径）：

```markdown
# OpenSkill-Code

- **本地路径**: `<CONFIRMED_PATH>`
- **激活规则**: 收到开发需求或项目查询时，读取 `<PATH>/CLAUDE.md` 获取可用 skill
- **按需读取**: 不预加载所有 skill，仅触发时读取对应 SKILL.md
- **无匹配降级**: 没有匹配 skill 时按常规方式处理
- **workspace**: 动态记忆层；AI 自主判断写入，但项目进度通常应更新，用户纠偏后的稳定结论优先沉淀并索引
- **沟通协议**:
  - 解释代码、设计、问题定位时，先用业务含义说明“这个模块是干什么的、问题影响什么”，再给类名、方法名、文件路径作为证据
  - 不要一上来堆英文类名；如果必须提类名，必须同步补一句中文业务解释
  - 输出优先按业务功能分组，不按类名罗列；让非代码视角的人也能先看懂结论
```

写入规则：

- 如果项目 `CLAUDE.md` 中已存在 `# OpenSkill-Code` 索引段，只更新其中路径和说明，**禁止重复追加**
- 如果项目 `CLAUDE.md` 不存在，则先创建，再写入索引段
- 如果无法确认项目根目录、无法创建或无法写入 `CLAUDE.md`，必须显式报错，不得跳过或假装安装成功

### Step 4: 完成

报告安装完成 + 最终路径 + 用户级 CLAUDE 注册结果 + 项目根目录。**此后不再读取本 README**，优先入口为用户级 `~/.claude/CLAUDE.md`，项目级 `CLAUDE.md` 作为项目内补充索引。

## Runtime Protocol

安装完成后，运行时遵循以下顺序：

1. 先读用户级 `~/.claude/CLAUDE.md`
2. 根据其中的 OpenSkill-Code 索引，跳转到 `<PATH>/CLAUDE.md`
3. 再按需读取当前项目根目录中的 `CLAUDE.md` 作为项目补充约束
4. 只按需读取命中的 `SKILL.md`
5. 只有在 `SKILL.md` 明确引用时，才继续读取对应 `references/`

## 目录结构

```
OpenSkill-Code/
  README.md          # 本文件 -- 安装引导（只读一次）
  CLAUDE.md          # 技能注册表 -- AI 每次激活时的入口
  workspace/         # workspace
    _global/         # 公共偏好与公共索引
    {project-name}/  # 项目级 workspace（按项目名分目录）
  skills/
    10-understand/   # 规范类
    20-plan/         # 编排类
    30-execute/      # 执行类
    40-feedback/     # 反馈类
```
