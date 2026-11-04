---
name: receive-review
description: Use when acting on code review feedback (from a reviewer subagent, the user, or a PR), before implementing the suggestions - verify each item against the codebase instead of agreeing blindly.
---

# Receive Review

Evaluate the feedback technically before implementing it. Correctness beats social comfort.

1. Read all the feedback first.
2. Check each item against the real code. Is it correct for this codebase, stack and platform? Does it break anything? Is there a reason the code is the way it is?
3. Implement in order: blocking issues (breaks, security), then simple fixes, then complex ones. Test each fix and check for regressions.

## By source

- **A reviewer subagent** (a finished report, nobody to reply to): verify each finding yourself. Fix it, or record it as `Open:` / `Deferred:` with your reason. Keep going without asking the user, unless a finding conflicts with a decision the user made.
- **The user:** trusted. Clarify unclear items before implementing any of them, since items are often related. Then implement.
- **An external reviewer (PR):** skeptical but careful. Clarify unclear items first. If you can't verify one, say "can't verify without X". Reply in each comment's own thread.

## Push back when

- the suggestion breaks existing behavior, or the reviewer lacks context
- it's YAGNI (grep for usage: "nothing calls this endpoint; remove it?")
- it's wrong for this stack, or legacy/compatibility reasons apply

Push back with evidence (code, tests, grep output). When replying, state the fix plainly ("Fixed: <what> in <file:line>") rather than praise.
