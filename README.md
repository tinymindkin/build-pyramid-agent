# Build Pyramid Agent

[中文说明](README.zh-CN.md)

Build Pyramid Agent is a portable instruction pack for designing AI agents with a three-layer configuration model:

- `SOUL.md` defines who the agent is.
- `PRINCIPLE.md` defines how the agent decides.
- `AGENTS.md` defines how the agent operates.

Use it with Claude Code, Codex, OpenCode, or any agent runner that can load markdown instructions.

## Get Started

Clone or copy this repository:

```bash
git clone git@github.com:tinymindkin/build-pyramid-agent.git build-pyramid-agent
cd build-pyramid-agent
```

Ask your agent to use the builder instructions:

```text
Read SKILL.md and use it to interview me, then create AGENTS.md, SOUL.md, and PRINCIPLE.md for my new agent.
```

For a faster first draft:

```text
Read SKILL.md and create a first draft for a personal coding agent. Ask only for missing details that materially change the files.
```

## Generated Files

- `SOUL.md`: identity, voice, relationship, personality boundaries, and interaction style.
- `PRINCIPLE.md`: decision heuristics, tensions, uncertainty handling, and regressions.
- `AGENTS.md`: mission, autonomy, approvals, tools, privacy, memory, and operating rules.

If your environment expects `PRINCIPLES.md`, use that filename instead of `PRINCIPLE.md`.

## Configure In Claude Code

Claude Code reads `CLAUDE.md`, not `AGENTS.md`. Keep the shared files and add a small Claude adapter:

```bash
cat > CLAUDE.md <<'EOF'
@AGENTS.md
@SOUL.md
@PRINCIPLE.md
EOF
```

For project-level usage, commit `CLAUDE.md`, `AGENTS.md`, `SOUL.md`, and `PRINCIPLE.md` to the project root.

For personal global usage, put reusable preferences in:

```text
~/.claude/CLAUDE.md
```

Claude also supports `.claude/rules/` for scoped rules when one file becomes too large.

Reference: [Claude Code memory docs](https://docs.anthropic.com/en/docs/claude-code/memory)

## Configure In Codex

Codex reads `AGENTS.md` files automatically.

Project-level:

```bash
cp AGENTS.md /path/to/project/AGENTS.md
cp SOUL.md PRINCIPLE.md /path/to/project/
```

Add this near the top of `AGENTS.md` if you keep the layers separate:

```md
Before working, read `SOUL.md` and `PRINCIPLE.md` in this directory if they exist. Treat them as identity and decision guidance for this agent.
```

Global defaults:

```bash
mkdir -p ~/.codex
cp AGENTS.md ~/.codex/AGENTS.md
```

Reference: [Codex AGENTS.md docs](https://developers.openai.com/codex/guides/agents-md)

## Configure In OpenCode

OpenCode reads `AGENTS.md` from the project root and `~/.config/opencode/AGENTS.md` globally.

Project-level:

```bash
cp AGENTS.md /path/to/project/AGENTS.md
cp SOUL.md PRINCIPLE.md /path/to/project/
```

To make OpenCode load the separated layers automatically, add `opencode.json`:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["SOUL.md", "PRINCIPLE.md"]
}
```

Global defaults:

```bash
mkdir -p ~/.config/opencode
cp AGENTS.md ~/.config/opencode/AGENTS.md
```

Reference: [OpenCode rules docs](https://opencode.ai/docs/rules/)

## Configure In Other Agents

Use this fallback pattern:

1. Put `AGENTS.md` wherever the tool expects project instructions.
2. Put `SOUL.md` and `PRINCIPLE.md` beside it.
3. If the tool only loads one file, paste short summaries of `SOUL.md` and `PRINCIPLE.md` into `AGENTS.md`.
4. If the tool supports extra instruction files, register `SOUL.md` and `PRINCIPLE.md` as additional context.

## Project Structure

```text
build-pyramid-agent/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── README.md
└── README.zh-CN.md
```

## Model

- Skills tell an agent what to do.
- Principles tell an agent how to decide.
- Soul tells an agent who to be.

Keep the files alive. When the agent fails, record the regression and update the smallest file that would prevent the same failure next time.
