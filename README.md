# agent-rules

**TL;DR:** Personal Claude Code rules, grouped by folder. `build.sh` combines the folders into three profiles: local, cloud, and cloud dev environment. Source of truth for `~/.claude/CLAUDE.md` on every machine that clones this repo to `~/Development/agent-rules`.

## Decisions / actions needed

- Re-paste `CLAUDE.inline.md` into Claude Desktop custom instructions: approval gates replaced auto-mode write confirmation and self-review before commit. Cloud sessions pick this up after merge to `main`.

## Layout

| Path | Purpose |
|---|---|
| `core/` | Rules for every environment: chat brevity, plain English, doc brevity, evidence |
| `interactive/` | Rules for sessions with a human present: approval gates (ask at plan, commit, push/PR, and risky or external writes; review loop between), clarify-first |
| `dev/` | Rules for software dev tasks, local or cloud: dev standard (TDD, PR size limit, quality) |
| `autonomous/` | Rules for unattended runs in the cloud dev environment: unattended run (only active when the first message starts with `UNATTENDED RUN`) |
| `build.sh` | Generates `CLAUDE.md`, `CLAUDE.inline.md` and `CLAUDE.dev.md`. `./build.sh --check` fails if they are out of date. CI runs it on every push and pull request |
| `CLAUDE.md` | **Generated.** Local profile: imports `core/`, `interactive/` and `dev/` |
| `CLAUDE.inline.md` | **Generated.** Cloud profile: `core/` + `interactive/`, rule text inlined |
| `CLAUDE.dev.md` | **Generated.** Cloud dev environment profile: all four folders, rule text inlined |

To add or change a rule:

- Edit or add a `.md` file in the right folder. Folders load in the order `core/`, `interactive/`, `dev/`, `autonomous/`; files load in alphabetical order inside each folder.
- Run `./build.sh`.
- Commit the rule file together with the regenerated `CLAUDE.md`, `CLAUDE.inline.md` and `CLAUDE.dev.md`.
- Keep `CLAUDE.md` plus all its imports under 150 lines in total.

## Usage

**Claude Code (local CLI / Desktop app local session / Remote Control)** — create a symlink from `~/.claude/CLAUDE.md` to the generated file. Its imports point straight into this repo, so no other links are needed. The repo must live at `~/Development/agent-rules` (set in `build.sh` as `REPO_HOME`).

    ln -sf ~/Development/agent-rules/CLAUDE.md ~/.claude/CLAUDE.md

One-time migration on machines set up before the folder layout: remove the old per-rule links, which now point at missing files.

    rm -f ~/.claude/rules/{brevity-chat,plain-english,brevity-docs,evidence,auto-mode-writes,clarify-first,self-review-before-commit,dev-standard}.md

**Claude Code on the web (cloud sessions)** — each cloud session starts from a fresh clone of the repo, so your local `~/.claude/CLAUDE.md` is not there. In the cloud environment settings, add this line to the setup script. (In the Desktop app: environment dropdown → **Add environment** → **Setup script**.)

    mkdir -p ~/.claude && curl -fsSL https://raw.githubusercontent.com/wz185/agent-rules/main/CLAUDE.inline.md -o ~/.claude/CLAUDE.md

This writes your user-level rules file outside the repo, so nothing gets committed. `-f` makes the script fail instead of saving an error page as the rules file.

The setup script result is cached as a snapshot for about 7 days. After a rule change is merged, edit and save the setup script (any change, such as a comment with the date) so new sessions get the new rules.

**Cloud dev environment (unattended runs)** — a separate cloud environment for dev tasks. It needs a Jira connector that works without a login prompt, and push access to the GitHub repo. Use this setup script instead of the one above:

    mkdir -p ~/.claude && curl -fsSL https://raw.githubusercontent.com/wz185/agent-rules/main/CLAUDE.dev.md -o ~/.claude/CLAUDE.md

Schedule a session in that environment with a prompt that starts with `UNATTENDED RUN`, for example:

    UNATTENDED RUN. Pick one ready Jira ticket labelled claude-ready-wei, build it, open a draft PR, and end with the report.

Only a first message that starts with `UNATTENDED RUN` turns on unattended mode. Sessions you start yourself in the same environment use the normal approval gates.

**Claude Desktop app (chat, not Claude Code)** — doesn't read local files. Paste the contents of `CLAUDE.inline.md` (or a single file from `core/` or `interactive/`) into Settings → Profile → custom instructions, or into a Project's custom instructions.
