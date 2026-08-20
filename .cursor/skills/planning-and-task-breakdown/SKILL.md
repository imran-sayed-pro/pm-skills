---
name: planning-and-task-breakdown
description: >-
  Decomposes a spec into small, verifiable tasks with explicit acceptance
  criteria, a dependency graph, vertical feature slices, and checkpoints.
  Use when a spec needs to be broken into implementable units, a task feels
  too large, where to start work is unclear, work must be parallelized across
  agents or sessions, scope must be communicated to a human, implementation
  order is not obvious, or the user mentions planning, task breakdown, task
  list, or /build. Do not use for a single-file change with obvious scope, or
  when the spec already contains well-defined tasks.
---

# Planning and task breakdown

A good task breakdown is the difference between an agent that works reliably and one that produces a tangled mess. Every task should be small enough to implement, test, and verify in a single focused session. Agents perform best on **S** and **M** tasks.

## When to use

- You have a spec and need to break it into implementable units.
- A task feels too large.
- Where to start work is unclear.
- Work needs to be parallelized across multiple agents or sessions.
- You need to communicate scope to a human.
- Implementation order is not obvious.

## When not to use

- Single-file changes with obvious scope.
- The spec already contains well-defined tasks.

## Task list target

Define the target **once**, then every other reference in this skill defers to that default.

| Target | When |
|---|---|
| `task/todo.md` | Default. Checklist Markdown. This is the convention `/build` and other downstream tooling expect. Use it unless the project says otherwise. |
| External tracker (GitHub Issues, Jira, Linear) | If project agent rules (`AGENTS.md`, `CLAUDE.md`, or similar) or the user designate an issue tracker, create **one tracker item per task** instead of writing `task/todo.md`. |

If the project names other paths (this repo: `docs/plans/<slug>.md` and `docs/tasks/<slug>.md`), those win.

Create the `task/` directory if it does not exist and you are using the default paths.

---

## Process

### 1. Enter plan mode before writing any code

Switch to plan mode. Operate read-only. Do not write implementation code during planning.

- Read the spec and relevant codebase sections.
- Identify existing patterns and conventions.
- Map dependencies between components.
- Note risks and unknowns.

Output:

1. Plan document — always Markdown. Default path: `task/plan.md`.
2. Task list — recorded in the task list target defined above.

### 2. Identify the dependency graph

Map what depends on what. Example chain:

```
database schema → API models/types → API endpoints → frontend API client → UI components
```

Dependents cannot start until their dependency exists.

### 3. Slice vertically

Build one complete feature path at a time. Do not build all of the database, then all of the API, then all of the UI.

If features share an API contract: define the contract first, then parallelize.

### 4. Write each task

Same structure whether it lands in Markdown or an external tracker.

```markdown
### Task <id>: <title>

- Short description:
- Description: (one paragraph explaining what this task accomplishes)
- Acceptance criteria:
  - (specific, testable bullets)
- Test table:

  | Case | Input | Expected |
  |---|---|---|
  | | | |

- Verification:
  - Test pass: (full command with flags)
  - Build succeeds: (full command with flags)
  - Manual check:
- Dependencies: Task <id>, …  |  none
- Files likely touched:
- Estimated scope: XS | S | M | L | XL
```

Tracker mapping: title → title; description, acceptance criteria, test table, and verification → body; dependencies → the tracker’s linking mechanism. If a field has no natural equivalent, note it in `task/plan.md`.

### 5. Order, size, and checkpoints

Arrange tasks so dependencies are satisfied. Build the foundation first. High-risk tasks are early — fail fast.

Add an **explicit checkpoint** to the task list after every two to three tasks, and between major phases. Record checkpoints as tracker items too, or as a checklist in the plan document.

**Sizing**

| Size | Files | Scope | Example |
|---|---|---|---|
| XS | 1 | Single function or config change | Add a validation rule |
| S | 1–2 | One component or endpoint | Add a new endpoint |
| M | 3–5 | One feature slice | User registration flow |
| L | 5–8 | Multi-component feature | Search with filtering and pagination |
| XL | 8+ | Too large | Break it down further |

If a task is **L or XL**, break it into smaller tasks. **No task may touch more than five files.**

Break a task down further when:

- It would take more than one focus session (roughly 2+ hours of agent work).
- You cannot describe the acceptance criteria in three or fewer bullet points.
- It touches more than one independent subsystem (for example authentication and billing).
- You find yourself writing **AND** in the task title.
- It does not map cleanly onto two individual task issues — split until it does.

---

## Plan document template

Save to `task/plan.md` (or the project’s plan path).

```markdown
# Implementation plan — <feature or project name>

## Overview

<one-paragraph summary of what we are building>

## Architecture decisions

- <key decision and rationale>

## Task list

### Phase 1: Foundation

- Task 1: …
- Task 2: …
- Checkpoint: foundation tests pass; build clean

### Phase 2: Core features

- Task 3: …
- Task 4: …
- Checkpoint: core features; end-to-end flow works

## Risks and mitigation

-

## Open questions

None.

## Parallelization

**Safe to parallelize:** independent feature slices; tests for already-implemented features; documentation.

**Must stay sequential:** database migrations; shared state changes; dependency chains; anything that needs coordination.

**Shared contract:** define the contract first, then parallelize.
```

---

## Red flags — stop

- Starting implementation without a written task list
- Writing `task/todo.md` when the project has designated an external tracker
- Tasks that say "implement the feature" without acceptance criteria
- No verification steps in the plan
- All tasks are XL-sized
- No checkpoints between tasks
- Dependency order is not considered

---

## Verification before starting implementation

Do not leave planning until all of these are true:

- [ ] Every task has acceptance criteria
- [ ] Every task has a verification step (test pass, build succeeds, manual check)
- [ ] Task dependencies are identified and ordered correctly
- [ ] Tasks are recorded in the task list target (default `task/todo.md`)
- [ ] No task touches more than five files
- [ ] Checkpoints exist between major phases
- [ ] The human has reviewed and approved the plan

## Additional resources

- For a filled-in S-sized task, see [examples.md](examples.md)
