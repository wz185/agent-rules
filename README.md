# agent-rules

**TL;DR:** Personal Claude Code / Claude Desktop configuration rules — brevity, doc formatting, evidence standards, auto-mode write confirmation, and self-review-before-commit. Source of truth for `~/.claude/CLAUDE.md` across machines.

## Files

| File | Purpose |
|---|---|
| `CLAUDE.md` | Root import file — pulls in the rules below |
| `brevity.md` | Chat response formatting (lead with answer, bullets, no preamble) |
| `docs.md` | Markdown doc structure (TL;DR, decisions section, section limits) |
| `evidence.md` | Requires inline evidence for confident claims; no-evidence protocol |
| `auto-mode-writes.md` | Confirm before every write/mutate action in auto mode |
| `self-review-before-commit.md` | Review diffs before committing; never chain commit + push |

Note: `CLAUDE.md`'s `@~/.claude/rules/...` imports currently cover `brevity`, `docs`, `evidence`, and `auto-mode-writes` only — `self-review-before-commit.md` is not yet wired into the import list (kept here in sync with what's on disk at `~/.claude/rules/`).

## Usage

**Claude Code (local CLI / Desktop app local session / Remote Control)** — all three run on your own machine and auto-load `~/.claude/CLAUDE.md` already, so no action is needed. To make this repo the live source, symlink instead of copying:

```bash
ln -sf ~/Development/agent-rules/CLAUDE.md ~/.claude/CLAUDE.md
for f in brevity docs evidence auto-mode-writes self-review-before-commit; do
  ln -sf ~/Development/agent-rules/$f.md ~/.claude/rules/$f.md
done
```

**Claude Code on the web (cloud sessions)** — cloud sandboxes only ever see the repo being worked on ("repo only"; no local config). There's no built-in way to apply personal rules across every cloud session. Workaround: add a setup script to that project's cloud environment config that layers your rules on top of (not over) the project's own `CLAUDE.md`, using `CLAUDE.local.md` (an additive, most-specific memory layer, conventionally gitignored):

```bash
git clone https://github.com/wz185/agent-rules.git /tmp/agent-rules
cp /tmp/agent-rules/CLAUDE.md ./CLAUDE.local.md
```

Do **not** `cp` over `./CLAUDE.md` directly — that would overwrite the project's own conventions file if one exists.

**Claude Desktop app (chat, not Claude Code)** — doesn't read local files or Drive automatically; paste the relevant file's contents into Settings → Profile → custom instructions (global) or a Project's custom instructions (per-project).

## Decisions / actions needed

- None currently — update this section if a rule changes in a way that needs re-pasting into Claude Desktop, or if `self-review-before-commit.md` gets added to `CLAUDE.md`'s import list.
