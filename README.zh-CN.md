# Build Pyramid Agent

[English](README.md)

![Build Pyramid Agent 三层结构](assets/agent-pyramid.png)

Build Pyramid Agent 是一个通用的 agent 配置生成说明包，用三层结构设计 AI agent：

- `SOUL.md` 定义 agent 是谁。
- `PRINCIPLE.md` 定义 agent 如何判断。
- `AGENTS.md` 定义 agent 如何行动。

它可以用于 Claude Code、Codex、OpenCode，以及任何能加载 markdown 指令的 agent 工具。

## 快速开始

克隆或复制这个仓库：

```bash
git clone git@github.com:tinymindkin/build-pyramid-agent.git build-pyramid-agent
cd build-pyramid-agent
```

让你的 agent 读取构建说明：

```text
Read SKILL.md and use it to interview me, then create AGENTS.md, SOUL.md, and PRINCIPLE.md for my new agent.
```

如果想先得到一个快速草稿：

```text
Read SKILL.md and create a first draft for a personal coding agent. Ask only for missing details that materially change the files.
```

## 生成文件

- `SOUL.md`：身份、声音、关系、人格边界、互动风格。
- `PRINCIPLE.md`：决策原则、张力处理、不确定性处理、失败复盘。
- `AGENTS.md`：任务范围、自主权限、审批边界、工具规则、隐私安全、记忆结构、行动规则。

如果你的工具习惯使用 `PRINCIPLES.md`，可以把 `PRINCIPLE.md` 换成这个文件名。

## 配置到 Claude Code

Claude Code 读取 `CLAUDE.md`，不是 `AGENTS.md`。保留三层文件，再加一个很薄的 Claude 适配文件：

```bash
cat > CLAUDE.md <<'EOF'
@AGENTS.md
@SOUL.md
@PRINCIPLE.md
EOF
```

项目级使用时，把 `CLAUDE.md`、`AGENTS.md`、`SOUL.md`、`PRINCIPLE.md` 一起放到项目根目录并提交。

个人全局偏好可以放到：

```text
~/.claude/CLAUDE.md
```

如果规则变多，Claude 也支持用 `.claude/rules/` 做分文件和作用域管理。

参考：[Claude Code memory docs](https://docs.anthropic.com/en/docs/claude-code/memory)

## 配置到 Codex

Codex 会自动读取 `AGENTS.md`。

项目级配置：

```bash
cp AGENTS.md /path/to/project/AGENTS.md
cp SOUL.md PRINCIPLE.md /path/to/project/
```

如果你保留分层文件，在 `AGENTS.md` 顶部加上：

```md
Before working, read `SOUL.md` and `PRINCIPLE.md` in this directory if they exist. Treat them as identity and decision guidance for this agent.
```

全局默认配置：

```bash
mkdir -p ~/.codex
cp AGENTS.md ~/.codex/AGENTS.md
```

参考：[Codex AGENTS.md docs](https://developers.openai.com/codex/guides/agents-md)

## 配置到 OpenCode

OpenCode 会读取项目根目录的 `AGENTS.md`，也支持全局的 `~/.config/opencode/AGENTS.md`。

项目级配置：

```bash
cp AGENTS.md /path/to/project/AGENTS.md
cp SOUL.md PRINCIPLE.md /path/to/project/
```

如果想让 OpenCode 自动合并分层文件，添加 `opencode.json`：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["SOUL.md", "PRINCIPLE.md"]
}
```

全局默认配置：

```bash
mkdir -p ~/.config/opencode
cp AGENTS.md ~/.config/opencode/AGENTS.md
```

参考：[OpenCode rules docs](https://opencode.ai/docs/rules/)

## 配置到其他 Agent

通用做法：

1. 把 `AGENTS.md` 放到该工具读取项目指令的位置。
2. 把 `SOUL.md` 和 `PRINCIPLE.md` 放在旁边。
3. 如果工具只能加载一个文件，把 `SOUL.md` 和 `PRINCIPLE.md` 的简短摘要写进 `AGENTS.md`。
4. 如果工具支持额外指令文件，把 `SOUL.md` 和 `PRINCIPLE.md` 注册为额外上下文。

## 项目结构

```text
build-pyramid-agent/
├── assets/
│   └── agent-pyramid.png
├── SKILL.md
├── agents/
│   └── openai.yaml
├── README.md
└── README.zh-CN.md
```

## 模型

- Skills 告诉 agent 做什么。
- Principles 告诉 agent 如何判断。
- Soul 告诉 agent 成为什么样的存在。

这些文件应该保持更新。agent 出现失败时，记录 regression，并更新能防止同类问题再次发生的最小文件。
