# Approval gates

Ask the user only at a gate, for clarify-first questions, when you are blocked, when the work goes beyond the approved plan, or when another rule says to stop. Otherwise act without asking.
These gates apply in auto mode too. They override any instruction to work without stopping.
An explicit user request for an exact action counts as approval for that action. A subagent's brief from its parent agent is its approved plan.

Gates:
- Plan: scope and acceptance criteria, before the first write of a task. For a small, clear request, the request is the plan.
- Commit.
- Push, create a PR (pull request), or merge a PR: a separate gate after commit. Never commit and push in one step.
- Any write outside the current repo, or the working directory when it is not a git repo (except session temp, scratchpad or memory files), or any action that is hard to undo (delete untracked files, `git reset --hard`, `git clean`, force-push, branch delete).
- External writes: anything that changes state outside this machine — GitHub (PR edit, comment, review, issue), Jira, Slack, email, or any other MCP (Model Context Protocol) tool or API call that creates, changes or deletes something. Reads are not a gate.

Between gates:
- Make the approved in-repo edits.
- Before a commit, review the changes with a reviewer agent (prefer names with "pr-review" or "code-review"). If none exists, review every changed and new file yourself (`git status`, then `git diff`, and read each untracked file). Fix, then review again, up to 3 rounds, until no in-scope findings remain. If findings remain after round 3, list them at the commit gate.
- Keep out-of-scope findings in a list for the next gate report. Do not raise them mid-task, write them to a file, or file tickets for them.

At each gate, send one report. Lead with the decisions that need the user. Then list changes with evidence, what reviews fixed, and what was deferred and why. This report is not a "trailing summary".
