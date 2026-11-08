# Plan status rules

The single source for how plan.md is kept current. Used by `lite-powers:execute-plan` and `lite-powers:subagent-dev`. The plan file has to be correct even if the session dies mid-task, so write each update as soon as it happens.

## Per step

Tick a finished step or acceptance line: `- [x]`. A step that didn't happen stays `- [ ]`, with the reason on the same line: `- [ ] Verify ... (skipped: no test runner)`.

## Per task

- When a task starts: `**Status:** in progress`.
- When it ends, set one of these statuses: `done` | `partial: <what's missing>` | `blocked: <why>` | `skipped: <why>`.
- Commit the task's code first. Then fill in **Result:** with:
  - commits `abc1234..def5678`
  - test command and its result
  - `Deviation: <what> - <why>` for each departure from the task text
  - subagent mode: the review verdict, plus `Open: <finding>` / `Deferred: <finding>` lines
- Commit the plan update as its own small commit (`docs: plan task N`), git only.

## Plan header

- `**Base:**`: written once, when execution starts (`git rev-parse HEAD`). It survives compaction and is the start of the final review's range.
- `**Status:**` draft -> approved -> in progress -> done | stopped. Set `stopped` (and mark the current task `blocked: <why>`) before pausing to ask the user anything.

## At the end: Outcome

Fill in `## Outcome`:
- **Done:** what now works
- **Not done / partial:** each item with its reason
- **Deviations:** collected from the tasks
- **Review:** fixes made from the final review, and the final suite result
- **Open issues / deferred findings**
- **Follow-ups**

Then link the plan's Outcome from brainstorm.md's **Next** line, and update the roadmap.md row if there is one.

## Resume

After compaction, reread plan.md and continue at the first task whose Status isn't `done`, at its first unticked step. The plan file (including **Base**) and `git log` are the memory. A header with `**Unattended:** yes` means the run keeps following [unattended.md](../unattended/unattended.md).
