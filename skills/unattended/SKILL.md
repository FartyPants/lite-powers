---
name: unattended
description: Draft a /goal condition that runs an approved plan to the end without the user present, deferring hand checks and recording blockers.
argument-hint: "plan path, optional task range"
disable-model-invocation: true
---

# Unattended

Turn an approved plan into a `/goal` condition the user pastes to start an unsupervised run. This skill writes the goal and starts nothing. The run itself follows [unattended.md](unattended.md).

## Gather

Read the plan named in the argument and the design.md, brainstorm.md and roadmap.md beside it. A plan still in `draft` needs approval first: say so and stop. For a plan already `in progress` or `stopped`, the goal covers the tasks whose Status isn't `done`.

Take these from the files:

- **Tasks:** the range in the argument, otherwise all of them.
- **Execution and Testing:** the plan header. If either is missing, ask now in one AskUserQuestion, because the run can't ask later.
- **Final checks:** the full-suite command, plus any smoke check the plan names (the app starts, the server answers).
- **Spec:** the paths the run must follow.
- **Branch:** the current one. On main/master of a repo with a remote, a new feature branch named after the topic.
- **Hand checks:** every Acceptance or Verify line only a person can judge (heard, seen on a real display, felt, needs their account or hardware). Name each one with its task.
- **Boundaries:** from the Constraints and design: paths the run must not touch, and what it may install. If the plan names no installs, the rule is "Install nothing the plan doesn't name."

## Write the goal

Fill in this template. Keep project specifics in, drop any line with nothing to say, and include the roadmap rule only when roadmap.md exists. `/goal` accepts at most 4,000 characters, so check the length and shorten the wording of project-specific lines if it runs over.

The goal check reads only the conversation and runs no tools, so every "Done means" line has to be provable from what the run prints. Each line must also be reachable when a task ends up blocked, or the goal never clears.

```text
Execute <plan path>, Tasks <range>, with lite-powers:<execute-plan | subagent-dev> (<Native | Subagent-driven>, <Plain | TDD>), unattended.

Done means:
- Every task in range has a final Status in plan.md and its Result filled in.
- <full-suite command> ran after the last fix and passes, or its failures are listed in Outcome. <Smoke check, same form.>
- verification.md lists every hand check that remains for me.
- A final fresh review of the whole change has run and its findings are fixed or listed.
- The last message shows each task's Status, the tail of the full-suite output, the verification.md path (or that no hand checks remain) and the review verdict.

Rules:
- Do not stop to ask me. Follow <spec paths>. For a small decision, make the conventional choice and record it in plan.md.
- Hand checks need me (<Task N "what">; ...). Do everything else in those tasks, list them in verification.md, and continue.
- If a task is truly blocked after two different fix attempts, mark it blocked with the evidence, then continue with any task that doesn't depend on it.
- Never mark an acceptance line done without running its check. Report failures as failures.
- Work on branch <branch>. Commit per task as the plan says. Never push, publish or merge.
- Take no destructive, irreversible or security-sensitive action. Mark that task blocked instead.
- Do not change anything in <protected paths> or the approved design. If the design turns out to be wrong, record the problem and stop that task.
- Install nothing beyond <what plan.md names>. If something else is needed, mark the task blocked.
- Do not start any other roadmap sub-project.
```

## Hand it over

Print the goal in one code block, with the plan's hand checks and boundaries visible for a last read. Then tell the user to switch to auto mode and run `/goal` with that text. `/goal` keeps the current permission mode, so in any other mode the run can stall on a prompt.
