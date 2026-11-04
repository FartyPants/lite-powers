# Implementer prompt template

Dispatch with `subagent_type: general-purpose` and an explicit `model`. Fill `[TDD_SKILL_PATH]` with the absolute path of `../tdd/SKILL.md`, resolved from this skill's base directory.

```
Implement [TASK]: either "Task N of the plan at <plan path>" (read that task,
its Interfaces and the plan's Constraints) or "the small change described in
<brainstorm.md path>". The exact values are there. Spec: [SPEC_PATH or none].

Context you can't read there: [earlier decisions / deviations touching this work]
Testing mode: [Plain | TDD]   Work in: [directory]

If requirements or approach are unclear, ask now.

1. Build it to its Acceptance lines (or the brainstorm's decisions).
2. TDD mode: call the Skill tool with `lite-powers:tdd` (if that skill is
   unavailable, read [TDD_SKILL_PATH] instead) and follow it at the agreed
   seams. Plain mode: run the Verify command and read the output.
   Then run the project's full test command if one exists.
3. Commit if the task has a commit step.
4. Reread your diff for gaps and extras, and fix what you find.

The coordinator owns plan.md and the review. Report to it, and keep all the
work in this agent. If the task needs an architectural decision the plan
didn't make, report BLOCKED with the options.

Reply in 10 lines or fewer:
- Status: DONE | DONE_WITH_CONCERNS | NEEDS_CONTEXT | BLOCKED
- Acceptance: which lines are met; any unmet, with the reason
- Deviations, with the reason
- Commits (short sha + subject)
- Tests: command and result (TDD: the RED failure line and the GREEN pass line)
- Concerns or blockers, specifically
```
