---
description: Gate 1 — clarify requirements and write a complete spec before any plan or code.
alwaysApply: true
---

# Gate 1 — Spec

Start from the human’s high-level vision. Ask clarifying questions until requirements are understood. Do not guess product intent. Do not skip questions because the answer seems obvious.

If the request bundles several independently testable capabilities, do not write a module spec. Write `docs/capability-map.md` first with Module ID, capability, dependency direction, and build order. Wait for human approval. Then write one spec per module. Every module spec must trace to a Module ID in the approved map. One spec must not own several independently testable capabilities.

Write `docs/specs/<slug>.md` covering all of:

1. **Objective** — what, why, for whom, and what success looks like
2. **Project structure** — source, tests, documents
3. **Code style** — one real snippet showing naming and formatting (not prose-only)
4. **Testing strategy** — framework, test locations, coverage, three-tier system (unit, integration, end-to-end). If naming how to run tests or a build, write the full executable command with flags, not a tool name.
5. **Engineering practices** — run tests before commits; follow naming conventions; validate input; ask first before schema changes, new dependencies, or CI config changes; never commit secrets (API keys, passwords)
6. **Success criteria** — specific, testable conditions
7. **Open questions** — unresolved items needing human input, or an explicit statement that none remain

Also define in-scope and out-of-scope boundaries.

Do not leave Gate 1 until: all six core areas plus Open questions are present; success criteria are testable; boundaries are defined; the spec is saved in the repo; the human has approved it; and if capabilities were bundled, the capability map was approved first and every module spec traces to a Module ID. Then commit the spec.
