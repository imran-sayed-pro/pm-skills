---
description: Gate 2 — write a technical plan from an approved spec before tasks or code.
alwaysApply: true
---

# Gate 2 — Plan

Input: a validated, human-approved spec. If the spec is missing or unapproved, return to Gate 1.

Write `docs/plans/<slug>.md`. Follow `.cursor/skills/planning-and-task-breakdown/SKILL.md` (this repo uses `docs/plans/<slug>.md`, not `task/plan.md`). The plan must:

1. Identify major components and their dependencies
2. Determine implementation order — what must be built first
3. Include risk mitigation strategies
4. Identify what can be built in parallel vs what must be sequential
5. Define verification checkpoints between phases
6. Write every install, generate, test, build, and verify step as a full executable command with flags. Tool names alone are not allowed. Wrong: `vitest`. Right: `npx vitest run --coverage --coverage.provider=v8 tests/unit/projection.test.ts`.

Do not start Gate 3 until this plan is written and the human has approved it.
