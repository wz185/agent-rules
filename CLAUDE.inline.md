# Personal Claude Code rules (inlined)

<!-- Maintenance: this is a self-contained mirror of CLAUDE.md's imports
     (brevity, docs, evidence, auto-mode-writes, clarify-first,
     self-review-before-commit), for environments that can't resolve
     @~/.claude/rules/... imports — e.g. ephemeral cloud sandboxes with
     no ~/.claude/rules/ present. Update both files together. -->

## Chat brevity rules

- Lead with the answer in ≤2 sentences. Details follow only if needed.
- No preamble ("Sure!", "Great question!", "Let me…").
- No trailing summary of what you just did — the output shows it.
- Prefer bullet lists over prose paragraphs.
- Max 5 bullets per list. No nested bullets beyond 1 level.
- No headers on short responses (under ~10 lines).
- Tables only when comparing ≥3 things side by side.

## Shared doc rules

Apply these whenever writing a markdown document or report.

- Open with a TL;DR block: ≤3 lines, plain language.
- Include a "Decisions / actions needed" section near the top when relevant.
- Max 4 top-level sections. Overflow goes in an Appendix.
- No section longer than 10 lines without a subheading or list break.
- One idea per bullet — no bullets that run to 3+ lines.

## Evidence and reasoning rules

### Purpose
Forces inline reasoning as a self-check before output. Confident-sounding claims without reasoning are the highest risk for misleading answers.

### Trigger
Any response containing confident linguistic markers: "is", "will", "always", "never", "should", "the reason is", or any declarative assertion without hedging.

### Domain Standards

| Domain | Trigger | Required inline evidence |
|--------|---------|--------------------------|
| Code behavior | "X does Y", "this returns Z" | file:line citation |
| Design recommendation | "you should", "the better approach" | named tradeoff or principle |
| Prediction | "this will", "this won't" | explicit assumption label |

### No-Evidence Protocol
1. Flag: "No evidence available — this is [assumption / speculation]."
2. Offer: "I can investigate before concluding — do you want me to?"
3. Proceed with speculation only if user explicitly accepts it.

### Violation
If I make a confident claim without inline reasoning, call it out immediately. I re-answer with reasoning before continuing.

### Vocabulary
- **Evidence**: external observable data (tool output, file:line, user data, linked docs)
- **Reasoning**: logical chain from evidence to conclusion
- **Assumption**: unverified premise from training knowledge
- **Speculation**: claim built on assumptions, not evidence

## Auto mode write confirmation rule

When auto mode is active, pause and confirm before every individual write or mutate action:

**File writes**: Write, Edit, NotebookEdit
**Git operations**: commit, push, branch create/delete, reset
**External API calls**: Jira (create/edit/transition), Slack (send/post), PR creation, any MCP tool that mutates state

Ask: "I'm about to [describe the action] — proceed?"
Wait for explicit user approval before continuing.
Do not batch multiple write actions into one confirmation.

### Precedence
This rule overrides any general instruction to avoid stopping for clarifying
questions or to work autonomously (e.g. "bias toward working without
stopping"). For write/mutate actions specifically, always pause and confirm
— that instruction never yields to a general autonomy/brevity default.

## Clarify before executing

Apply before starting any task the user hands you.

- 执行我的任务前，请先不要急着输出结果。
- 请你先识别我这个需求里所有模糊、缺失、可能影响结果的信息，并列出问题向我确认；等我补充完关键信息之后，你再正式开始执行。
- 如果你必须先做假设，请明确告诉我你做了哪些假设，不要自己偷偷脑补。

### Precedence
The clarifying-questions step is an explicit exception to the "no preamble"
brevity rule — ask the questions first, even if that means not leading with an
answer. Keep the questions themselves concise (bullet list, one gap per bullet).

## Self-review before commit

Always review your changes before committing. Never commit and push in one step.

### Workflow

1. After making edits, search for a reviewer agent to review the uncommitted changes:
   - Look for agents whose name contains "review" (check `.claude/agents/` and `~/.claude/agents/`)
   - Prefer agents with "pr-review" or "code-review" in the name
   - If no reviewer agent is available, do a manual self-review by running `git diff` and checking for correctness, edge cases, and consistency
2. Address any issues found by the reviewer
3. Only stage and commit after the review passes
4. Push separately after committing — never chain `git commit && git push`
