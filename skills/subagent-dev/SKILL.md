---
name: subagent-dev
description: Use when the user chose Subagent-driven execution for a plan or a small change - a fresh subagent implements each task, a fresh reviewer checks it, then one final whole-change review.
---

# Subagent-Driven Development

You coordinate; subagents implement and review. Each implementer sees only its task, so your context stays free for coordination. You are the only one who edits plan.md, following [plan-status.md](../write-plan/plan-status.md).

## Before starting

- If the user hasn't chosen Native vs Subagent-driven for this work, ask (AskUserQuestion, options as in `lite-powers:write-plan`). Native -> switch to `lite-powers:execute-plan`.
- **With a plan:** read it and its spec, and note the Constraints and the **Testing** mode (ask Plain vs TDD if missing). Write `**Base:** <git rev-parse HEAD>` and `**Status:** in progress` into the plan header.
- **Small change (no plan):** the brainstorm.md is the single task. Record Base under its **Execution / Testing** line, run the per-task loop once (the review in step 4 is also the final review), and put the Result into its **Outcome**.
- Work on a feature branch when the repo is shared; main/master needs the user's OK.
- Resuming? Follow the Resume rule in plan-status.md.

**Model:** set `model` on every dispatch.
- Default to the session model.
- A cheaper model is fine for mechanical slices (1-2 files, clear interfaces).
- On BLOCKED, escalate to a stronger model.

## Per task

Run one implementer at a time, in plan order, respecting **Blocked by**. Batch small same-shape edits across files into one dispatch.

1. Record the task's start: `git rev-parse HEAD`. The review range runs from there to HEAD, which covers multi-commit tasks. Set the task Status to `in progress`.
2. Dispatch an implementer with [implementer.md](implementer.md). Give it pointers (plan path and task number, or the brainstorm.md path) plus only what it can't read there: decisions from earlier tasks and any Deviations that touch its interfaces.
3. Act on its status:
   - DONE: go to review.
   - DONE_WITH_CONCERNS: resolve correctness concerns first.
   - NEEDS_CONTEXT: answer, then resume the same agent.
   - BLOCKED: add context, use a stronger model, or split the task. Something changes before any retry.
4. Review: dispatch a reviewer with [reviewer.md](../request-review/reviewer.md) over the task's range. Requirements are the task's Acceptance lines plus the Constraints.
5. Handle the findings:
   - Critical/Important: resume the implementer with the findings verbatim, for one fix round. Re-check the fix diff, yourself or with a short reviewer pass. Anything still open becomes an `Open:` line.
   - Minor: record as `Deferred:` lines.
6. Update plan.md from the report: ticks, Status, Result (commits, tests, deviations, review verdict, Open/Deferred). Commit it.

Keep going through all tasks without check-ins. The pause conditions are the same as in `lite-powers:execute-plan`; set the plan Status to `stopped` before pausing.

## Final review (plans)

One reviewer, on the strongest model available, over `<Base>..HEAD`, with the plan, the spec, "Risks to check" and every Open/Deferred line. For Critical/Important findings, dispatch one fixer with the whole list, then run the full suite yourself. That's the last fix wave; anything still failing goes into Outcome.

## Done when

- Every task has a final Status and a filled-in Result.
- The full suite ran green after the last fix, or its failures are listed in Outcome.
- The final review findings are fixed or recorded.
- Outcome is filled in, the plan Status is `done`, and brainstorm.md links the Outcome.

Final message: a short summary and a link to the Outcome. Merging and pushing stay with the user.
