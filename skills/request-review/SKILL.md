---
name: request-review
description: Use after finishing a feature or a plan, or before merging, to get a fresh-context code review of the change from a reviewer subagent.
---

# Request Review

A fresh reviewer catches what the author can't see.

1. Pick the range. `BASE` is the commit before the work started (recorded earlier, or `git merge-base main HEAD`); `HEAD` is now. With no commits, review the working tree against BASE (`git diff BASE`).
2. Dispatch a `general-purpose` subagent with [reviewer.md](reviewer.md). Fill in:
   - `[WHAT]`: a one-line summary of what was built
   - `[REQUIREMENTS]`: the plan/spec path or task text, plus the constraints
   - `[RANGE]`: the range from step 1
   - `[FOCUS]`: the plan's "Risks to check", or "none"
3. Set `model`: the session model by default; the strongest available model for whole-feature reviews.
4. Act on the result with `lite-powers:receive-review`:
   - Critical: fix now.
   - Important: fix before moving on.
   - Minor: list it for the user.
   - The reviewer is wrong: push back with evidence.
