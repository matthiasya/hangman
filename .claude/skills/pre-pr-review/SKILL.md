---
name: pre-pr-review
description: Mix-of-experts review of the current branch before opening a PR — Claude /code-review plus Codex code and test reviews via herdr. Use before every PR in this repo.
---

# Pre-PR review

Run this on a feature branch once the change is done, before pushing and opening a PR.

## 1. Gate: tests and lint

Run the test suite and the linter/formatter (commands are in CLAUDE.md). Everything must pass before review starts — reviewers should not spend time on failures the tools already catch.

## 2. Collect reviews

Get three independent reviews of the diff against `main`:

1. **Claude — code:** run `/code-review` on the branch.
2. **Codex — code:** correctness, design, readability.
3. **Codex — tests:** are these the *right* tests, not just many tests.

### Reaching Codex (herdr)

Only when `HERDR_ENV=1`. Otherwise skip to "Not in herdr" below. Follow the `herdr` skill for command details.

Find the Codex agent — don't hardcode a pane ID:

```bash
herdr agent list   # pick the entry with "agent": "codex"; use its pane_id (or name, if set)
```

If its status is not `idle`/`done`, it is busy with the user's own work: tell the user and wait, don't interrupt it. If no Codex agent exists, ask the user before starting one.

Codex may be running in a different cwd, so always give it the absolute repo path. Send the two reviews one at a time, reading each result before sending the next:

```bash
herdr agent prompt <codex> "<prompt>" --wait --timeout 600000
herdr agent read <codex> --source recent-unwrapped --lines 300
```

Code review prompt:

> Review the changes on branch `<branch>` against `main` in the repo at `<abs repo path>` (`git diff main...HEAD`). Do not edit any files. Report only actionable findings — bugs, design problems, unclear code, missing error handling — each with file:line, severity (high/medium/low) and a one-line fix suggestion. Say "no findings" if there are none.

Test review prompt:

> Review only the tests changed or added on branch `<branch>` against `main` in the repo at `<abs repo path>`. Do not edit any files. Judge whether these are the right tests: do they check behaviour rather than implementation details; which realistic bugs in the changed code would they miss; which edge cases are untested; which tests are redundant, brittle or test nothing meaningful. Report each finding with file:line and severity. Say "no findings" if there are none.

### Not in herdr

The remote workflow (desktop/mobile apps) is not decided yet. Run the Claude review, then ask the user how they want the Codex reviews done. Do not open the PR without them unless the user says to.

## 3. Reconcile

Merge the three sets of findings into one list:

- **Agreed or uncontested and clearly correct:** fix them.
- **Reviewers disagree** (with each other, or with your own judgement): do **not** settle it yourself. Show the user both positions briefly and ask.
- **Out of scope for this PR:** don't fix; offer to file a GitHub issue.

After fixes, re-run tests and lint. Re-review only if the fixes were substantial.

## 4. Report

Give the user a short summary: what each reviewer found, what you fixed, what needs their call. Then open the PR once the open points are settled.
