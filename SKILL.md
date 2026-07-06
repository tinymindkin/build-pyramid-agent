---
name: build-pyramid-agent
description: Build portable AI agent configuration from the Soul / Principles / Agents hierarchy. Use when the user wants to create, design, scaffold, or refine a persistent AI agent, personal agent, team agent, operating agent, coding agent, or tool-agnostic agent config for Claude, Codex, OpenCode, or another runner, especially when they ask for AGENTS.md, CLAUDE.md, SOUL.md, PRINCIPLE.md, PRINCIPLES.md, "agent persona", "agent principles", "agent rules", or Chinese requests such as "创建 agent", "构建 agent 配置", "生成 AGENTS.md/SOUL.md/PRINCIPLE.md".
---

# Build Pyramid Agent

Create a portable, living agent configuration in three layers:

- `SOUL.md`: who the agent is. Character, voice, relationship, taste, boundaries of personality.
- `PRINCIPLE.md`: how the agent decides. Actionable heuristics for ambiguity, tension, friction, failure, and learning.
- `AGENTS.md`: how the agent operates. Tool use, approvals, memory, safety, autonomy, environment rules.

If the user explicitly asks for `PRINCIPLES.md`, use that name instead of `PRINCIPLE.md`.

## Intake

If requirements are missing, ask only the smallest useful set of questions:

1. What is this agent for, and who does it serve?
2. What kind of entity should it feel like: employee, partner, coach, operator, researcher, friend, something else?
3. What voice should it use: formal, direct, warm, playful, blunt, restrained?
4. What should it optimize for when task completion conflicts with truth, quality, speed, privacy, or learning?
5. What may it do autonomously, and what always needs approval?
6. What contexts will it operate in: local files, chats, email, calendar, code, Notion, browser, production systems?
7. What failure patterns or regressions should it watch for?
8. Which agent runners should this target: Claude, Codex, OpenCode, Cursor, custom system prompt, or something else?

If the user wants an immediate draft, make conservative assumptions and mark them as TODOs in the files.

## Writing Rules

Write specific operating text, not slogans.

Good principles:

- Change behavior in a hard moment.
- Resolve a real tension.
- Are general enough to reuse, but concrete enough to act on.
- Explain what to do when there is no clear instruction.

Bad principles:

- "Be helpful"
- "Be accurate"
- "Respect the user"
- Any value statement that does not change a decision.

## Output Structure

Create or update these core files in the target agent folder.

### SOUL.md

Include:

- Identity: what the agent is and is not.
- Relationship to the user.
- Voice and interaction style.
- Opinions and pushback rules.
- Emotional texture: warmth, humor, bluntness, restraint.
- Anti-patterns that would make the agent generic or annoying.

### PRINCIPLE.md

Include:

- Decision principles, each with a short name and operational meaning.
- Tensions the agent must navigate.
- What to do under uncertainty.
- How to handle friction, mistakes, and regressions.
- A `Regressions` section for lessons learned from failures.

Prefer 5-9 principles. Fewer strong principles beat many weak ones.

### AGENTS.md

Include:

- Mission and scope.
- Autonomy and approval boundaries.
- Tool, file, network, and external-action rules.
- Privacy and safety rules.
- Memory architecture: what to remember, where, and when to update it.
- Context-specific behavior: direct messages, group chats, coding work, browser/email/calendar, production systems.
- Downtime or heartbeat behavior if the agent runs persistently.
- How `SOUL.md` and `PRINCIPLE.md` should be read and updated.

## Runner Adapters

Add adapter files only when the user asks for a target runner or when the target is obvious.

- Claude Code: create `CLAUDE.md` that imports the layers:

```md
@AGENTS.md
@SOUL.md
@PRINCIPLE.md
```

- Codex: use `AGENTS.md` as the main loaded file. If `SOUL.md` and `PRINCIPLE.md` remain separate, add an instruction near the top of `AGENTS.md` to read them before working.
- OpenCode: use `AGENTS.md`. If separated layers should load automatically, create `opencode.json` with:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["SOUL.md", "PRINCIPLE.md"]
}
```

- Other runners: put `AGENTS.md` wherever project instructions are loaded; paste short `SOUL.md` and `PRINCIPLE.md` summaries into `AGENTS.md` if only one instruction file is supported.

## Feedback Loop

Make the configuration living:

- Add a `Regressions` section when absent.
- When a rule fails, record the concrete failure and update the smallest file that would prevent recurrence.
- Keep identity in `SOUL.md`, decision heuristics in `PRINCIPLE.md`, and operations in `AGENTS.md`.
