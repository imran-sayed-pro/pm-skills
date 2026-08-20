# Spec-driven framework — copy into another project

Copy the workshop machinery only. Do not copy this product’s specs, plan, tasks, source, or tests.

Set `DEST` to the other project’s root, then run the commands from **this** repo.

```bash
SRC="/Users/imransayed/Documents/the projects/project1"
DEST="/path/to/new-project"
```

## Copy commands

```bash
mkdir -p "$DEST"

rsync -a \
  --include='AGENTS.md' \
  --include='.cursor/' \
  --include='.cursor/rules/***' \
  --include='.cursor/skills/***' \
  --exclude='*' \
  "$SRC/" \
  "$DEST/"

mkdir -p "$DEST/docs/specs" "$DEST/docs/plans" "$DEST/docs/tasks"
```

UI-only skill (skip on non-UI projects):

```bash
rm -rf "$DEST/.cursor/skills/frontend-ui-engineering"
```

## What this copies

```
AGENTS.md
.cursor/rules/00-ignore-user-skills.mdc
.cursor/rules/01-spec-driven-gates.mdc
.cursor/rules/02-spec-gate.mdc
.cursor/rules/03-plan-gate.mdc
.cursor/rules/04-task-gate.mdc
.cursor/rules/05-build-gate.mdc
.cursor/skills/planning-and-task-breakdown/SKILL.md
.cursor/skills/planning-and-task-breakdown/examples.md
.cursor/skills/code-review-and-quality/SKILL.md
.cursor/skills/frontend-ui-engineering/SKILL.md
```

Empty dirs created (not copied with content):

```
docs/specs/
docs/plans/
docs/tasks/
```

## Do not copy

- `docs/capability-map.md`
- `docs/specs/` (`pfa-m1` … `pfa-m4`)
- `docs/plans/pfa-v1.md`
- `docs/tasks/pfa-v1.md`
- `src/`, `tests/`, `index.html`, `package.json`, Vite / Playwright / Vitest config
- User skills outside the repo (`~/.cursor/skills/`, plugins, marketplaces)

`docs/capability-map.md` is written in Gate 1 when a request bundles several capabilities.

## After copy (Cursor)

1. Keep `AGENTS.md` at the new repo root.
2. Open that folder as its own Cursor project so `.cursor/rules/` apply.
3. Start at Gate 1. Do not reuse this app’s specs.

## Claude Code

Claude Code reads `CLAUDE.md`, not `AGENTS.md`. It does not load `.cursor/rules/` or `.cursor/skills/` unless you bridge them. Keep `AGENTS.md` as the single source of truth.

| This repo (Cursor) | Claude Code |
|---|---|
| `AGENTS.md` | `CLAUDE.md` with `@AGENTS.md` at the top |
| `.cursor/rules/*.mdc` | `.claude/rules/*.md` (no `paths:` frontmatter = always on) |
| `.cursor/skills/*/SKILL.md` | `.claude/skills/*/SKILL.md` (same Agent Skills format) |

Do not duplicate the gate text into a second constitution. Import `AGENTS.md`. After import, tell Claude Code where *its* skills and rules live, and to ignore `~/.claude/skills/` unless you name a skill.

### Cursor + Claude Code in the same dest

Run the Cursor copy block first, then:

```bash
mkdir -p "$DEST/.claude/rules"

printf '%s\n' \
  '@AGENTS.md' \
  '' \
  '## Claude Code' \
  '' \
  'In-repo skills for this tool live under `.claude/skills/`.' \
  'Project rules for this tool live under `.claude/rules/`.' \
  'Ignore `~/.claude/skills/` unless the human names a skill in this conversation.' \
  'If an in-repo skill conflicts with the four-gate workflow in `AGENTS.md`, the workflow wins.' \
  > "$DEST/CLAUDE.md"

ln -sfn ../.cursor/skills "$DEST/.claude/skills"

for f in "$DEST/.cursor/rules/"*.mdc; do
  cp "$f" "$DEST/.claude/rules/$(basename "$f" .mdc).md"
done
```

`ln -sfn` fails on Windows without Developer Mode; copy instead:

```bash
rsync -a "$DEST/.cursor/skills/" "$DEST/.claude/skills/"
```

Confirm in a Claude Code session with `/context` that `CLAUDE.md` and `AGENTS.md` appear under Memory files.

### Claude Code only (no Cursor)

Same as above, but you can skip `.cursor/` if that project will never open in Cursor. You still copy `AGENTS.md` (the constitution) and the skill bodies:

```bash
mkdir -p "$DEST/.claude/skills" "$DEST/.claude/rules" \
  "$DEST/docs/specs" "$DEST/docs/plans" "$DEST/docs/tasks"

cp "$SRC/AGENTS.md" "$DEST/AGENTS.md"

printf '%s\n' \
  '@AGENTS.md' \
  '' \
  '## Claude Code' \
  '' \
  'In-repo skills for this tool live under `.claude/skills/`.' \
  'Project rules for this tool live under `.claude/rules/`.' \
  'Ignore `~/.claude/skills/` unless the human names a skill in this conversation.' \
  'If an in-repo skill conflicts with the four-gate workflow in `AGENTS.md`, the workflow wins.' \
  > "$DEST/CLAUDE.md"

rsync -a "$SRC/.cursor/skills/" "$DEST/.claude/skills/"

for f in "$SRC/.cursor/rules/"*.mdc; do
  cp "$f" "$DEST/.claude/rules/$(basename "$f" .mdc).md"
done
```

Optional: in that Claude-only repo, edit `AGENTS.md` so skill paths say `.claude/skills/` instead of `.cursor/skills/`.

### Do not rely on

- Claude Code reading `AGENTS.md` with no `CLAUDE.md` (it will not)
- `.cursor/rules/*.mdc` applying inside Claude Code
- User skills in `~/.claude/skills/` (this workshop is in-repo only)
- `/import` as the lasting setup — it copies once; later gate edits in `AGENTS.md` will not flow unless you use `@AGENTS.md`
