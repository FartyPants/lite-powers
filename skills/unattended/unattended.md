# Unattended run rules

Used when a plan runs under a `/goal` drafted by `lite-powers:unattended`. These rules replace every pause and question in the execution skills. Everything else in those skills and in [plan-status.md](../write-plan/plan-status.md) still applies, and the goal's own rules win over this file.

## At start

Write `**Unattended:** yes` into the plan header, next to **Base**, and work on the branch the goal names. The header line carries this mode across compaction.

## Decisions

- Never ask the user. Small decision: make the conventional choice and add `Decision: <choice> - <why>` to the task's Result.
- A subagent's NEEDS_CONTEXT gets the same treatment: answer from the plan and spec, or with the conventional choice.

## Where a normal run would pause

Mark the task `blocked: <why and evidence>`, leave the action undone, and continue with any task that doesn't depend on it. This applies to:

- destructive or irreversible actions
- security-sensitive actions
- outward side effects (push, publish, merge)
- anything to install that the plan doesn't name
- a design that turns out to be wrong: record the problem in the Result and leave the design as it is
- a failure still there after two different fix attempts, including a `lite-powers:debug` loop that can't be built

A task that depends on a blocked one becomes `blocked: depends on Task N`.

## Hand checks

Do everything in the task except the part only a person can judge. Leave that line as `- [ ] <line> (hand check: verification.md)` and add it to `verification.md` beside plan.md. A task whose only open lines are hand checks is `done`, with `Hand check pending: <what>` in its Result.

```markdown
# Verification: <plan name>

Hand checks that remain for you. Everything else was run and is recorded in plan.md.

- [ ] Task N: <what to do>. Expected: <what you should see or hear>. Start from: <command, build or file>.
```

Commit verification.md with the plan update.

## Evidence

Tick an Acceptance line only after running its check in this run. A failure is reported as a failure, with its output.

The goal check reads only the conversation. End each task with a line giving its number, Status and test result. Commits and these lines also show the run is still making progress.

## Ending

Run the final review over whatever was built, even with blocked tasks. Write the Outcome. Set the plan Status to `done` when every task in the plan is `done`, otherwise `stopped`.

The last message gives each task's Status, the tail of the full-suite output, the verification.md path (or that no hand checks remain) and the review verdict. Merging and pushing stay with the user.
