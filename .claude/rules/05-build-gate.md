---
description: Gate 4 — execute one task at a time and keep the spec in sync with reality.
alwaysApply: true
---

# Gate 4 — Build

Execute one approved task at a time. Inspect relevant files first, then implement. Do not implement anything that is not in the spec and the current task.

Keep the spec alive. It is a living document, not a one-time artifact.

- If a decision changes (for example the data model), update the spec first, then implement.
- If scope changes (features added or cut), update the spec, plan, and task list to match, then continue.
- After updating the spec, commit it. The spec belongs in version control alongside the code.

Pull requests must link back to the spec section each PR implements.

Before every commit: run the current task’s Test command and Build exactly as written (full executable with flags). Never commit secrets, including API keys and passwords. Ask first before database schema changes, adding dependencies, or changing CI config.
