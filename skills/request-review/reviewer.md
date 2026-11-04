# Reviewer prompt template

Used by `lite-powers:request-review`, `lite-powers:execute-plan` (final review) and `lite-powers:subagent-dev` (task and final reviews).

```
Review a code change. Keep the checkout untouched: read with git diff, git
show and git log, and do the whole review yourself.

What was built: [WHAT]
Requirements: [REQUIREMENTS]
Range: [RANGE]   (git diff --stat [RANGE]; git diff [RANGE])
Focus: [FOCUS]

Standards: read CLAUDE.md / AGENTS.md, CONTRIBUTING.md, CODING_STANDARDS.md,
GLOSSARY.md and ADRs if they exist. Cite the rule for each standards finding.
A documented repo rule overrides your own taste. Skip anything linters or
formatters already enforce.

Read the diff. Look outside it only to check a specific risk you can name,
and say which risk and what you checked.

Check:
- Spec: anything missing, extra (unrequested), or misunderstood? Quote the
  requirement for each finding.
- Correctness: bugs, edge cases, error handling, security.
- Tests: do they assert behavior at public seams, with independent expected
  values? Are the edge cases covered?
- Structure: one responsibility per file, consistent with the codebase,
  names that say what things do.
Where the spec is silent, judge by what a reasonable user would expect.

Grade by real effect:
- Critical: bugs, data loss, security, broken function.
- Important: missed requirement, fragile behavior, swallowed errors, tests
  that assert nothing, a documented-standard breach.
- Minor: polish.

Output, starting directly with the first heading:
### Spec: compliant | issues: <missing/extra/misunderstood, file:line>
### Critical
### Important
### Minor
Each finding: file:line, what's wrong, why it matters, fix if not obvious.
### Set aside
One line per behavior you judged out of scope, with the reason (or "none").
### Verdict: Ready | Ready with fixes | Not ready - <1-2 sentence reason>
```
