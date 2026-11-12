# Dev standard

Applies to any software dev task (feature, bug fix, refactor) in any repo.

## Precedence and autonomy
- Repo instructions win on code conventions: stack, style, branching, commit and PR format. This standard wins on process.
- Work up to a local commit, reviewed as in self-review-before-commit. Then stop and report. In auto mode, auto-mode-writes still applies.
- Never commit to, push to, or rebase `master`, `main` or the default branch without explicit user approval. This overrides repo branching rules.
- Never push, open a PR (pull request), or merge a PR without explicit user approval. Never deploy to production or change its data or config.

## Task size
- Write acceptance criteria as a checklist before coding.
- One task = one behaviour change, testable alone, one small PR. Split bigger work into slices first; each slice delivers value or sits behind a feature flag.

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
- At any point, stop and ask if the requirement is unclear, tests cannot run, the task grows beyond one slice, or the change touches DB migrations, infra, auth or billing.
