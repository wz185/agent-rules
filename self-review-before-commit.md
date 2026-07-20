---
paths:
  - "**/*"
---

# Self-review before commit

Always review your changes before committing. Never commit and push in one step.

## Workflow

1. After making edits, search for a reviewer agent to review the uncommitted changes:
   - Look for agents whose name contains "review" (check `.claude/agents/` and `~/.claude/agents/`)
   - Prefer agents with "pr-review" or "code-review" in the name
   - If no reviewer agent is available, do a manual self-review by running `git diff` and checking for correctness, edge cases, and consistency
2. Address any issues found by the reviewer
3. Only stage and commit after the review passes
4. Push separately after committing — never chain `git commit && git push`
