# hangman

A hangman game in Python. Terminal (CLI) first; a web UI may follow later.

## Architecture

- Keep the game logic (word state, guesses, win/lose rules) in a pure module with no I/O.
- The CLI is a thin layer over that logic, so a web UI can reuse it unchanged.
- Prefer the Python standard library. Ask before adding a third-party dependency.

## Commands

Not set up yet: the first code PR chooses the test runner and linter/formatter and lists the commands here.

## Workflow

1. **Spec first.** Before writing a spec or starting a new feature, run `/grill-me` to work through the idea with the user.
2. **Branch.** Branch from an up-to-date `main`. `main` is protected: every change lands through a PR, with no direct or force pushes.
3. **Small, focused PRs.** Keep each PR to one change.
4. **Tests with every change.** New or changed behaviour comes with tests. Aim for the *right* tests, not many: test behaviour rather than implementation, cover edge cases, and catch realistic bugs.
5. **Pre-PR review.** Before opening a PR, follow the `pre-pr-review` skill: tests and lint must pass, then a Claude `/code-review` plus Codex code and test reviews. When reviewers disagree, bring it to the user. Don't settle it yourself.
6. **Open the PR** once review findings are resolved.

## Work tracking

- Track work in GitHub Issues (`gh issue`). Turn specs from `/grill-me` sessions into issues.
- Reference the issue in each PR (`Closes #<n>`).
- File out-of-scope review findings and technical debt as issues.

## Environments

- **Home (herdr):** Claude and Codex run in separate herdr panes. Reach Codex with the `herdr` skill. Check `HERDR_ENV=1` first.
- **Remote (Claude/Codex desktop and mobile apps):** the review workflow is TBD. Don't assume Codex is reachable; ask the user.

## Engineering standards

Code quality and automated test coverage matter most here. Work like a principal engineer:

- Apply SOLID, DRY, YAGNI and KISS pragmatically. Don't over-engineer a small game.
- Write clean, readable code that tells a story and keeps cognitive load low.
- Follow the test pyramid: mostly fast unit tests on the game logic, a few CLI-level tests.
- State assumptions and edge cases explicitly when analysing requirements.
- When you take on or spot technical debt, say so and offer to file a GitHub issue.

## Communication

Keep explanations short and crisp. No long summaries after every step.
