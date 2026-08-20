---
description: Enforce Spec → Plan → Task → Build. Do not skip gates or write code early.
alwaysApply: true
---

# Four gates

Work strictly in order. Do not enter a later gate until the current gate is complete and human-approved. Do not write implementation code until Gate 4.

1. Spec — clarify, then write the spec
2. Plan — technical implementation plan from the approved spec
3. Task — discrete, dependency-ordered tasks
4. Build — execute one task at a time

Default paths: `docs/capability-map.md`, `docs/specs/<slug>.md`, `docs/plans/<slug>.md`, `docs/tasks/<slug>.md`.

# Red flags — stop

- Writing code without a written, approved requirement
- Offering to “just build” before clarifying what
- Architectural decisions not documented in the spec or plan
- Features not in any spec or task list
- Skipping a gate, question, test, or verification because it seems obvious
- Naming a tool in a plan, task, or checkpoint without the full command and flags
