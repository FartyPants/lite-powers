---
name: handoff
description: Compact the current conversation into a handoff document for a fresh session to pick up.
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

Write a handoff document so a fresh agent can continue this work. Save it to the OS temp directory (`%TEMP%` on Windows, `$TMPDIR` or `/tmp` elsewhere) and print its path.

- Point to existing artifacts (docs/work/ brainstorm, design and plan files, commits, diffs) by path instead of restating them. The plan's task statuses already say what's done.
- Capture what lives only in this conversation: decisions not yet written down, dead ends tried, the current hypothesis, the exact next step.
- Name the lite-powers skills the next session should load.
- Redact secrets and personal data.
- If the user passed an argument, tailor the document to that next focus.
