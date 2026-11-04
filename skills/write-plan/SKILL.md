---
name: write-plan
description: Use when there is an approved design or clear requirements for a multi-step coding task and an implementation plan is needed before touching code.
---

# Write Plan

Plan at the **decision level**. The implementer is a capable model: give it the decisions it can't make alone (task boundaries, files, interfaces, seams, acceptance criteria, exact values the spec pins) and let it write the code. The plan is also the living record of what was done; execution skills update it per [plan-status.md](plan-status.md).

## Step 0: ask the testing style

Before writing each plan, ask (AskUserQuestion), even if the user chose for an earlier plan:

- **Plain**: each task is implement, then verify (existing tests, build, or a concrete check).
- **TDD**: each task is built test-first at the agreed seams with `lite-powers:tdd`.

## Shape the tasks

- Read the design's seams and interfaces. If `GLOSSARY.md` or ADRs exist, use their vocabulary.
- If the spec covers independent subsystems, propose one plan per subsystem, tracked in a `roadmap.md`.
- Each task is a **vertical slice**: a narrow but complete path through every layer it needs (schema, logic, UI, tests). It's verifiable on its own and fits one context window. Prefactoring that makes later slices easy comes first.
- Exception: a wide mechanical refactor (rename, retype) goes expand -> migrate in batches -> contract, keeping every step green.
- Map files per task: what gets created or modified, each with one responsibility. Follow existing patterns.

Save as `plan.md` in the topic's existing `docs/work/<date>-<topic>/` folder, next to brainstorm.md and design.md. Create a new dated folder only for a topic that has none (user preference overrides).

```markdown
# <Feature> Plan

**Status:** draft
**Goal:** one sentence
**Approach:** 2-3 sentences
**Spec:** <link to design.md>   **Brainstorm:** <link>
**Execution:** Native | Subagent-driven   **Testing:** Plain | TDD
**Base:** <commit sha when execution starts; the final review diffs from here>

## Constraints
<project-wide requirements, exact values copied from the spec, one per line>

## Risks to check
<up to 5 inputs or failure modes the spec implies but no task covers; add a check for each to the owning task>

---

### Task N: <name>
**Status:** todo   **Blocked by:** <task numbers, or none>
**Delivers:** <the end-to-end behavior this slice makes work>
**Files:** Create `path` / Modify `path`
**Interfaces:** Consumes <names/signatures from earlier tasks>; Produces <names/signatures later tasks rely on>
**Tests at:** <seam(s) from the design; Plain mode: the verify command>
**Acceptance:**
- [ ] <checkable criterion, exact values where the spec pins them>

- [ ] Implement
- [ ] Verify: `<command>` -> <expected>
- [ ] Commit: `<message>` (git only)

**Result:**

---

## Outcome
```

Code appears in a plan only where prose would be ambiguous: an algorithm the interface doesn't determine, or exact copy the spec fixes. A task is ready when the implementer could build it without asking, and a reviewer could check it against Acceptance.

## Self-check (do it yourself)

1. Every spec requirement maps to an Acceptance line in some task.
2. Interface names match between Produces and Consumes.
3. Every Acceptance line is checkable; every Blocked by points at an earlier task.

Fix inline.

## Handoff: always ask the execution mode

Link the plan, then ask in one AskUserQuestion call, every time:

- **Native**: I implement every task in this session, then one fresh reviewer checks the whole change. Cheaper and faster.
- **Subagent-driven**: a fresh subagent implements each task and a fresh reviewer checks it, then one final review. More thorough; costs a context per task.

Add a one-line recommendation (task count, how coupled the tasks are), and ask whether the plan captures what they want.

When they answer, write **Execution** and `**Status:** approved` into the plan. Approving a plan with commit steps authorizes those commits; pushing stays with the user.

Then: Native -> `lite-powers:execute-plan`. Subagent-driven -> `lite-powers:subagent-dev`.
