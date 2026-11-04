---
name: brainstorm
description: Use when the user wants to build or change something non-trivial and what they want, or why, is not yet clear from the request. Quick, clear tasks go straight to work.
---

# Brainstorm

Find out what the user actually wants, and why, before building. Understanding the why lets you make the small decisions they never mention the way they would.

## First: is anything missing?

- Everything that matters is in the request: just do it. No docs needed.
- Something non-trivial is missing: start the conversation. Open with one line saying they can say "just build it" to skip the questions.

## The conversation: rounds

Ask in rounds, following [rounds.md](rounds.md): the whole frontier each round, numbered, each question with a recommended answer. Facts are yours to look up; decisions are theirs. One difference from a grilling: put only **load-bearing** decisions to the user (ones that change what gets built). State reversible ones as your defaults, marked "(my call)" in brainstorm.md. Approving the description is the shared-understanding confirmation.

- If `GLOSSARY.md` or ADRs exist, use their vocabulary and respect them. When a project term gets resolved or a hard-to-reverse decision is made, use `lite-powers:domain-model`.
- Use plain language and their vocabulary. Prefer open questions; when they say "just guess", propose something concrete.
- Ground to cover (territory, not a script): what it is, why now, what success looks like, non-goals, prior art, current state, how-choices (platform, libraries) they want to make vs leave to you. Build on what existing work already uses, and say your defaults out loud.
- Think of several approaches privately; show a comparison only when it helps them decide.
- **Spike** when a quick throwaway build is the cheapest way to learn. Spikes skip plans and reviews. When done, commit the spike to a throwaway branch (if git) and record the question it answered and the verdict in brainstorm.md.

## Write it down: `brainstorm.md`

For anything beyond a quick task, keep the record at `docs/work/YYYY-MM-DD-<topic>/brainstorm.md` (user preference overrides the location). Create it after the first round of answers and rewrite sections in place as decisions settle.

```markdown
# <Topic>: brainstorm
**Date:** YYYY-MM-DD   **Size:** small change | project | multi-project
**Next:** <link to design.md / plan.md / roadmap.md, or "build in chat">

## Request
<the user's ask, in their words>
## Why / success looks like
## Decisions
- <decision> (user)
- <decision> (my call: <reason>)
## Non-goals
## Approaches considered
<the chosen one and why; the rejected ones in one line each>
## Spikes
<question -> verdict, branch name>
## Open questions
## Execution / Testing
<Native | Subagent-driven> / <Plain | TDD>
## Outcome
<small changes only: what was built, what was not, test results, follow-ups>
```

## Size the work

Say the size in plain words; they can override.

| Size | Then |
|------|------|
| Quick, clear task | do it |
| Small change | brainstorm.md is the description; ask the two questions below; build; fill in **Outcome** |
| Project | `design.md`, then `lite-powers:write-plan` |
| Multi-project | `roadmap.md`, then brainstorm each sub-project in order |

**Small change, before building:** ask both questions in one AskUserQuestion call:
- **Execution:** Native (I implement it here) / Subagent-driven (a subagent implements, a fresh reviewer checks; run via `lite-powers:subagent-dev` with brainstorm.md as the task).
- **Testing:** Plain (implement, then run existing tests/build) / TDD (failing test first; uses `lite-powers:tdd`).

## `design.md` (projects)

Same folder. Write it so a capable builder could plan from it without asking the user again:
- Intent and why.
- Goals, non-goals, constraints.
- Decisions the user made, as they made them, and your calls marked as yours.
- Interfaces and contracts between modules.
- **Testing decisions / seams:** the public boundaries where behavior gets tested, preferring existing seams and as few as possible, with a line each on what the seam catches and what it misses. The seams are approved together with the design.
- What's left to the builder.

Describe modules and interfaces by name, not by file path; paths go stale and belong in plan.md.

Reread it once as that builder. Put any questions whose answer changes what gets built into the same message that asks for design approval.

## `roadmap.md` (multi-project)

`docs/work/YYYY-MM-DD-<topic>/roadmap.md` holds the overall goal and one row per sub-project: order, one-line scope, link to its folder, and a status that mirrors its plan's header (todo / in progress / done / stopped).

## Gate

Build after the user approves the description: brainstorm.md for a small change, design.md for a project. Reading, exploring and approved spikes are fine before that, and "just build it" opens the gate immediately.

After approval, projects go to `lite-powers:write-plan`. If the repo is git, commit the docs together with the first commit of the work, and leave pushing to the user.
