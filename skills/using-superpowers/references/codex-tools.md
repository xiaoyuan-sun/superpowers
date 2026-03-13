# Codex 工具映射

技能使用 Claude Code 工具名称。当你在技能中遇到这些名称时，使用你平台的对应工具：

| 技能引用 | Codex 对应工具 |
|-----------------|------------------|
| `Task` 工具（派遣子代理） | `spawn_agent` |
| 多个 `Task` 调用（并行） | 多个 `spawn_agent` 调用 |
| 任务返回结果 | `wait` |
| 任务自动完成 | `close_agent` 释放槽位 |
| `TodoWrite`（任务跟踪） | `update_plan` |
| `Skill` 工具（调用技能） | 技能原生加载 — 直接遵循指令 |
| `Read`, `Write`, `Edit`（文件） | 使用你的原生文件工具 |
| `Bash`（运行命令） | 使用你的原生 shell 工具 |

## 子代理派遣需要 collab

添加到你的 Codex 配置（`~/.codex/config.toml`）：

```toml
[features]
collab = true
```

这会启用 `spawn_agent`、`wait` 和 `close_agent`，用于 `dispatching-parallel-agents` 和 `subagent-driven-development` 等技能。