---
name: execute-plan
description: Use when the user chose Native execution for a plan - you implement every task yourself in this session, then one fresh reviewer checks the whole change.
---

# Execute Plan (Native)

You implement the plan task by task, keep plan.md current per [plan-status.md](../write-plan/plan-status.md), then one fresh reviewer checks the whole change.

## Before starting

- If the user hasn't chosen Native vs Subagent-driven for this plan, ask (AskUserQuestion, options as in `lite-powers:write-plan`). Subagent-driven -> switch to `lite-powers:subagent-dev`.
- Read the plan and its spec. Note the Constraints and the **Testing** mode (ask Plain vs TDD if missing). TDD mode: load `lite-powers:tdd` now.
- Work on a feature branch when the repo is shared; main/master needs the user's OK.
- Write `**Base:** <git rev-parse HEAD>` and `**Status:** in progress` into the plan header (git only for Base).
- Resuming? Follow the Resume rule in plan-status.md.

## Per task

Take tasks in order, respecting **Blocked by**.

1. Reread the task. Exact values live in the plan, not in memory. Set its Status to `in progress`.
2. Build it to its **Acceptance** lines. Plain mode: implement, then run the Verify command and compare with Expected. TDD mode: the `lite-powers:tdd` loop at the task's seams.
3. If the output doesn't match:
   - Obvious cause (typo, missing import): fix it.
   - Unclear cause: use `lite-powers:debug` in plan mode (see its last section).
   - The plan is wrong (contradicts the spec, interface mismatch): make the smallest change that satisfies the spec and record a `Deviation:`.
4. Tick the steps and Acceptance lines as they complete. Commit the code, then write the Result and Status and commit the plan update.

Keep going through all tasks without check-ins. Pause only for:
- destructive or irreversible actions
- security-sensitive actions
- outward side effects (push, publish, merge)
- a plan so broken that every path forward is a guess

Before pausing, set the plan Status to `stopped`.

Send long test output to a file and read its tail.

## Final review

Run `lite-powers:request-review` on `<Base>..HEAD` (or the working tree against Base if there were no commits), with the plan, the spec and "Risks to check". Handle the findings with `lite-powers:receive-review`:
- Critical/Important: fix in one pass, verify each fix (TDD mode: failing test first), then run the full suite.
- Minor: record as deferred in the Outcome.
- One review round in total.

## Done when

- Every task has a final Status and a filled-in Result.
- The full suite ran green after the review fixes, or its failures are listed in Outcome.
- The review findings are fixed or recorded.
- Outcome is filled in, the plan Status is `done`, and brainstorm.md links the Outcome.

Final message: a short summary and a link to the Outcome. Merging and pushing stay with the user.
