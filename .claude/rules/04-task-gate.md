---
description: Gate 3 — break the approved plan into small, dependency-ordered tasks.
alwaysApply: true
---

# Gate 3 — Task

Input: the approved plan. If the plan is missing or unapproved, return to Gate 2.

Write `docs/tasks/<slug>.md`. Follow `.cursor/skills/planning-and-task-breakdown/SKILL.md` (this repo uses `docs/tasks/<slug>.md`, not `task/todo.md`). Each task must be completable in a single focus session. Order tasks by dependency, not perceived importance. No task may change more than five files. Include the skill’s fields: acceptance criteria, test table, verification, dependencies, files, estimated scope. Checkpoints after every two to three tasks.

Use this template for every task:

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
