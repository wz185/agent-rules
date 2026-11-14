# Dev standard

Applies to any software dev task (feature, bug fix, refactor) in any repo.

## Precedence and autonomy
- Repo instructions win on code conventions: stack, style, branching, commit and PR format. This standard wins on process.
- Approvals for commit, push and PR (pull request) come from approval-gates, or from unattended-run when it is active.
- Never commit to, push to, or rebase `master`, `main` or the default branch without explicit user approval. This overrides repo branching rules.
- Never merge a PR without explicit user approval. Never deploy to production, or change production data or config.

## Task size
- Write acceptance criteria as a checklist before coding.
- One task = one behaviour change, testable alone. Each slice delivers value or sits behind a feature flag.
- PR size limit: 400 changed lines (tests count; lockfiles, generated files and snapshots do not) and 15 files. Stricter repo rules win.
- Estimate the size in the plan. If it is over the limit, split the work into stacked draft PRs (at most 3). If it still does not fit, report the plan only, with no code.
- If the work passes the limit while you build, stop and report why.

## TDD (test-driven development) and coverage
- Red, green, refactor. Confirm each new test fails on its assertion, not on a setup or compile error, before writing code.
- Bug fix: first write a test that reproduces the bug.
- Cover happy path, edge cases and error paths. Changed lines: at least 80% coverage, or the repo's threshold if higher. If the repo has no coverage tool, say so.
- Add e2e (end-to-end) tests for user-facing flows and cross-service paths. If the repo has no e2e setup, say so.
- Test through public interfaces. Mock only external boundaries. Never weaken or delete a test to get a pass.
- Before commit, run all relevant unit, integration and e2e tests.

## Quality and observability
- Lint, type check and build pass with no new warnings. No dead code, debug prints, or TODOs without a ticket.
- Never swallow errors silently. Keep secrets and PII (personally identifiable information) out of code and logs.
- For breaking changes to public APIs, events and database (DB) schemas, follow the repo's rules.
- Add structured logs at boundaries and failures, with correlation IDs and enough context to debug.
- Add metrics or traces for new endpoints, consumers, jobs and external calls. Use the repo's existing telemetry library.

## Finish
- Check each acceptance criterion with evidence (test name, output, file:line).
- Report: changes, test results, coverage, risks, how to verify in production, next step that needs approval.
- At any point, stop and ask if the requirement is unclear, tests cannot run, the task grows beyond one slice, or the change touches DB migrations, infra or auth.
