# agent-rules

**TL;DR:** Personal Claude Code rules, grouped by folder. `build.sh` combines the folders into two profiles: local and cloud. Source of truth for `~/.claude/CLAUDE.md` on every machine that clones this repo to `~/Development/agent-rules`.

## Decisions / actions needed

- None currently. Update this section if a rule change needs re-pasting into Claude Desktop.

## Layout

| Path | Purpose |
|---|---|
| `core/` | Rules for every environment: chat brevity, plain English, doc brevity, evidence |
| `interactive/` | Rules for sessions with a human present: clarify-first, auto-mode write confirmation, self-review before commit |
| `autonomous/` | Rules for unattended dev work: dev standard |
| `build.sh` | Generates `CLAUDE.md` and `CLAUDE.inline.md`. `./build.sh --check` fails if they are out of date. CI runs it on every push and pull request |
| `CLAUDE.md` | **Generated.** Local profile: imports all three folders |
| `CLAUDE.inline.md` | **Generated.** Cloud profile: `core/` + `interactive/`, rule text inlined |

To add or change a rule:

- Edit or add a `.md` file in the right folder. Folders load in the order `core/`, `interactive/`, `autonomous/`; files load in alphabetical order inside each folder.
- Run `./build.sh`.
- Commit the rule file together with the regenerated `CLAUDE.md` and `CLAUDE.inline.md`.
- Keep `CLAUDE.md` plus all its imports under 150 lines in total.

## Usage

**Claude Code (local CLI / Desktop app local session / Remote Control)** — create a symlink from `~/.claude/CLAUDE.md` to the generated file. Its imports point straight into this repo, so no other links are needed. The repo must live at `~/Development/agent-rules` (set in `build.sh` as `REPO_HOME`).

    ln -sf ~/Development/agent-rules/CLAUDE.md ~/.claude/CLAUDE.md

One-time migration on machines set up before the folder layout: remove the old per-rule links, which now point at missing files.

    rm -f ~/.claude/rules/{brevity-chat,plain-english,brevity-docs,evidence,auto-mode-writes,clarify-first,self-review-before-commit,dev-standard}.md

**Claude Code on the web (cloud sessions)** — cloud sandboxes only see the repo being worked on. In that project's cloud environment settings, add this line to the setup script. (In the Desktop app: environment dropdown → **Add environment** → **Setup script**.)

    curl -sSL https://raw.githubusercontent.com/wz185/agent-rules/main/CLAUDE.inline.md -o ./CLAUDE.local.md

Claude Code reads `./CLAUDE.local.md` in addition to the project's `CLAUDE.md`. Projects usually list it in `.gitignore`, so it is not committed. Do **not** write to `./CLAUDE.md`: that would overwrite the project's conventions file.

**Claude Desktop app (chat, not Claude Code)** — doesn't read local files. Paste the contents of `CLAUDE.inline.md` (or a single file from `core/` or `interactive/`) into Settings → Profile → custom instructions, or into a Project's custom instructions.
