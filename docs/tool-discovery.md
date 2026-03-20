# 系统工具发现协议

首次使用构建/测试/部署等系统工具时，按以下顺序发现：

```
1. 尝试默认命令（mvn, npm, pytest, go test 等）
   └→ 成功 → 记录到用户级 ~/.claude/CLAUDE.md
   └→ 失败 → 继续

2. 尝试常见路径搜索
   └→ which/where 查找
   └→ 检查 sdkman/nvm/pyenv 等版本管理器
   └→ 检查 IDE 自带工具（如 IntelliJ Maven）
   └→ 成功 → 记录到用户级 ~/.claude/CLAUDE.md
   └→ 失败 → 继续

3. 询问用户
   └→ "找不到 {tool}，请提供路径或安装方式"
   └→ 用户回复后 → 记录到用户级 ~/.claude/CLAUDE.md
```

记录格式（写入 `~/.claude/CLAUDE.md`）：

```markdown
## {语言} 构建
- **{tool}**: `{完整路径或命令}`
- **{test-runner}**: `{完整命令}`
```

**已记录的工具后续直接使用，不再重复发现。**
