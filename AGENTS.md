# Agent instructions

This repository is a spec-driven software workshop.

**In-repo skills are allowed.** Read, follow, and invoke every Agent Skill (`SKILL.md`) under `.cursor/skills/` when it applies. Ignore every user-defined skill from Cursor, Claude, plugins, marketplaces, and home-directory skill caches (`~/.cursor/skills/`, `~/.cursor/skills-cursor/`, `~/.claude/skills/`, and similar). If an in-repo skill conflicts with the four-gate workflow below, the four-gate workflow wins.

Current in-repo skills:

| Skill | When |
|---|---|
| `.cursor/skills/planning-and-task-breakdown/SKILL.md` | Gate 2 (plan) and Gate 3 (task list). Outputs are `docs/plans/<slug>.md` and `docs/tasks/<slug>.md`, not `task/plan.md` / `task/todo.md`. |
| `.cursor/skills/code-review-and-quality/SKILL.md` | Before merging any change; after a feature, bug fix, or refactor; when reviewing code written by yourself, another agent, or a human. |

Add new skills only under `.cursor/skills/<name>/SKILL.md`. Once present, they are in force without a further AGENTS.md edit, unless they conflict with the gates.

Work strictly in order. Do not enter a later gate until the current gate is complete and approved. Do not write implementation code until Gate 4.

1. **Spec** — clarify, then write the spec
2. **Plan** — turn the approved spec into a technical plan
3. **Task** — break the plan into implementable tasks
4. **Build** — execute one task at a time

Default artifact paths (override only in the spec’s Project structure section):

- Capability map: `docs/capability-map.md`
- Spec: `docs/specs/<slug>.md`
- Plan: `docs/plans/<slug>.md`
- Tasks: `docs/tasks/<slug>.md`

---

## Gate 1 — Spec

Start from the human’s high-level vision. Ask clarifying questions until requirements are understood. Do not guess product intent. Do not skip questions because the answer seems obvious.

If the request bundles several independently testable capabilities, stop. Do not write a module spec yet. Write a capability map first (`docs/capability-map.md`) with:

- Module ID
- Capability name and one-line description
- Dependency direction (which modules depend on which)
- Build order

Wait for human approval of the map. Then write one spec per module. Every module spec must trace to a Module ID in the approved map. One spec must not own several independently testable capabilities.

Each spec must cover these sections:

### 1. Objective

What we are building, why, for whom, and what success looks like.

### 2. Project structure

Where source code lives, where tests live, where documents live.

### 3. Code style

One real code snippet that shows naming conventions and formatting rules for this project. Do not describe style only in prose.

### 4. Testing strategy

Framework, where tests live, coverage expectations, and the three-tier system (unit, integration, end-to-end). Name what each tier is responsible for. If the spec names how to run tests or a build, write the full executable command with flags, not a tool name.

### 5. Engineering practices

- Always run tests before commits.
- Follow the naming conventions in Code style.
- Validate all input at the boundary.
- Ask first before database schema changes, adding dependencies, or changing CI config.
- Never commit secrets, including API keys and passwords.

### 6. Success criteria

Specific, testable conditions that prove this is done. Vague goals are not allowed.

### 7. Open questions

Anything unresolved that needs human input. If none remain, say so explicitly.

### Spec verification — do not leave Gate 1 until all of these are true

- The spec covers all six core areas (Objective, Project structure, Code style, Testing strategy, Engineering practices, Success criteria) plus Open questions.
- Success criteria are specific and testable.
- Boundaries are defined (in scope and out of scope).
- The spec is saved as a file in the repository.
- The human has reviewed and approved the spec.
- If capabilities were bundled, the capability map (Module ID, dependency direction, build order) was approved before any module spec was written, and every module spec traces to a Module ID in that map.

Commit the spec. It belongs in version control alongside the code.

---

## Gate 2 — Plan

Input: a validated, human-approved spec. Output: `docs/plans/<slug>.md`.

Follow `.cursor/skills/planning-and-task-breakdown/SKILL.md`. In this repository the plan path is `docs/plans/<slug>.md`, not `task/plan.md`.

The plan must:

1. Identify major components and their dependencies.
2. Determine implementation order — what must be built first.
3. Include risk mitigation strategies.
4. Identify what can be built in parallel vs what must be sequential.
5. Define verification checkpoints between phases.
6. Write every install, generate, test, build, and verify step as a full executable command with flags. Tool names alone are not allowed. Wrong: `vitest`. Right: `npx vitest run --coverage --coverage.provider=v8 tests/unit/projection.test.ts`.

Do not start Gate 3 until the plan is written and the human has approved it.

---

## Gate 3 — Task

Input: the approved plan. Output: `docs/tasks/<slug>.md`.

Follow `.cursor/skills/planning-and-task-breakdown/SKILL.md`. In this repository the task list target is `docs/tasks/<slug>.md`, not `task/todo.md`. Do not write `task/todo.md` here.

Break the plan into discrete tasks. Each task must be completable in a single focus session. Order tasks by dependency, not by perceived importance. No task may require changing more than five files. Use the skill’s task structure (acceptance criteria, test table, verification, dependencies, files, scope). Checkpoints after every two to three tasks and between phases.

Every task uses this template (skill fields mapped onto this project’s verify block):

```markdown
### Task <id>: <title>

- Short description:
- Description: (one paragraph)
- Acceptance criteria:
  - (specific, testable bullets)
- Test table:

  | Case | Input | Expected |
  |---|---|---|
  | | | |

- Verification:
  - Test pass: (full executable with flags)
  - Build succeeds: (full executable with flags)
  - Manual check:
- Dependencies: Task <id> | none
- Files likely touched: (maximum five)
- Estimated scope: XS | S | M
```

Test command and Build must be full executable invocations with flags. Wrong: `vitest` or `vite build`. Right: `npx vitest run --coverage --coverage.provider=v8 tests/unit/projection.test.ts` and `npx tsc --noEmit && npx vite build --emptyOutDir`.

Do not start Gate 4 until the task list is written and the human has approved it.

---

## Gate 4 — Build

Execute one approved task at a time. Inspect the relevant files first, then implement. Do not implement features that are not in the spec or the current task.

Keep the spec alive. It is a living document, not a one-time artifact.

- If a decision changes (for example the data model), update the spec first, then implement.
- If scope changes (features added or cut), update the spec, plan, and task list to match, then continue.
- After updating the spec, commit it.

Pull requests must link back to the spec section each PR implements.

---

## Red flags — stop if you are about to do any of these

- Start writing code without a written, approved requirement.
- Ask whether to “just build” before clarifying what to build.
- Make architectural decisions without documenting them in the spec or plan.
- Implement features not mentioned in any spec or task list.
- Skip a gate, a clarifying question, a test, or a verification step because it seems obvious.
- Name a tool in a plan, task, or checkpoint without the full command and flags.
