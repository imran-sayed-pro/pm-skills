---
description: Use all skills in this repo's .cursor/skills/ folder. Ignore user skills from outside the repository.
alwaysApply: true
---

# Skills

**Use** every Agent Skill (`SKILL.md`) in this repository under `.cursor/skills/`, including:

- `.cursor/skills/planning-and-task-breakdown/SKILL.md` — Gate 2 and Gate 3. Outputs are `docs/plans/<slug>.md` and `docs/tasks/<slug>.md`, not `task/plan.md` / `task/todo.md`.
- `.cursor/skills/code-review-and-quality/SKILL.md` — before merging; after a feature, bug fix, or refactor; when reviewing code from yourself, another agent, or a human.

Do **not** read, invoke, or follow any Agent Skill defined **outside** this repository (`~/.cursor/skills-cursor/`, `~/.cursor/skills/`, `~/.claude/skills/`, plugins, marketplaces). Do not use fable-mindset, buildable, articulate, odyssey, financial-analyst, create-rule, or any other listed user skill unless the human explicitly names that skill in this conversation.

The four-gate spec-driven workflow in these project rules and `AGENTS.md` still governs. If an in-repo skill conflicts with that workflow, the workflow wins.
