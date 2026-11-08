# lite-powers

Lightweight planning, execution, review and TDD skills for Claude Code, built for modern models.

lite-powers keeps the good ideas of [superpowers](https://github.com/obra/superpowers) and [mattpocock/skills](https://github.com/mattpocock/skills) and drops the weight.

## Why

The goal is skills written for modern models, which are much smarter than the original skills anticipated. Superpowers was designed around weaker models: it forces skill invocation on every turn, hard-gates work behind brainstorming, spells out plans down to exact code, pre-empts rationalizations with long tables, and wraps every task in ledgers and multi-round review loops. Those guardrails made sense when the model couldn't be trusted to plan, verify or judge. Today they mostly cost tokens, time and your patience.

lite-powers assumes a capable model. It gives the model the decisions it can't make alone (what you want, where the tests go, how the work should run) and trusts it with the rest:

- Plans are decision-level, not transcripts of code.
- Rules are stated once, positively, without warnings repeated "just in case".
- Reviews are a single fresh-context pass, not loops.

- **Nothing is forced.** Skills fire from their descriptions, and quick, clear tasks just get done. The session-start note is one line.
- **You choose how work runs.** Before building, you're always asked **Native** (Claude implements in this session) vs **Subagent-driven** (a fresh subagent per task, each reviewed), and **Plain** vs **TDD**.
- **Everything is written down.** Brainstorms, designs and plans live in your project under `docs/work/`, and the plan records what was and wasn't done after every step.
- **Small.** About 800 lines across 15 skills, supporting files included. The superpowers skills it replaces come to about 2,350 lines in their SKILL.md files alone, and about 4,300 with their supporting files.

## Install

Claude Code:

```
/plugin marketplace add FartyPants/lite-powers
/plugin install lite-powers@lite-powers
```

or from a terminal:

```bash
claude plugin marketplace add FartyPants/lite-powers
claude plugin install lite-powers@lite-powers
```

Restart Claude Code after installing. If you also have `superpowers` or `mattpocock-skills` enabled, disable them (`claude plugin disable <name>`); their overlapping skills (tdd, review, brainstorming) compete with these.

## The workflow

```
brainstorm  ->  write-plan  ->  execute-plan    (Native)
                            ->  subagent-dev    (Subagent-driven)
                                    |
                     request-review / receive-review
```

Not every task needs every stage. A quick, clear request is done directly. A small change is brainstormed and built. A project gets a design doc and a plan.

## Skills

### Model-invoked (Claude reaches for these on its own)

| Skill | What it does |
|---|---|
| `brainstorm` | Finds out what you want and why. Asks in **rounds**: every answerable question at once, numbered, each with a recommended answer so "yes" accepts it. Looks up facts itself and asks you only for decisions. Sizes the work. |
| `write-plan` | Decision-level plan: vertical-slice tasks with files, interfaces, test seams, acceptance criteria and blocking edges. The model writes the code. Asks Plain vs TDD first and Native vs Subagent-driven at the end, every time. |
| `execute-plan` | Native mode: implements every task in the session, keeps plan.md current, then one fresh reviewer checks the whole change. |
| `subagent-dev` | Subagent-driven mode: one implementer subagent per task, a reviewer per task, one fix round, one final review. |
| `tdd` | Red-green-refactor in vertical slices, at **seams** agreed with you up front. Fires only when you picked TDD. |
| `debug` | Feedback-loop-first debugging: build a command that reproduces the bug before forming theories, then minimise, hypothesise, instrument, and fix with a regression test. |
| `request-review` | Dispatches a fresh-context reviewer that checks spec, correctness, tests and your repo's documented standards. |
| `receive-review` | Checks each review finding against the code before acting; pushes back with evidence. |
| `domain-model` | Keeps `GLOSSARY.md` and ADRs (`docs/adr/`) up to date as terms and hard-to-reverse decisions settle. |

### User-invoked (type them)

| Command | What it does |
|---|---|
| `/lite-powers:grill` | Relentless interview to stress-test a plan, decision or idea. |
| `/lite-powers:grill-docs` | The same, writing glossary terms and ADRs as they settle. |
| `/lite-powers:handoff` | Writes a handoff document for a fresh session. |
| `/lite-powers:claude-handoff` | Writes the handoff and starts a background Claude session with it (`claude --bg`). |
| `/lite-powers:research` | Background agent researches primary sources and saves cited findings as Markdown. |
| `/lite-powers:unattended` | Drafts a `/goal` that runs an approved plan to the end without you: hand checks go to `verification.md`, blockers are recorded, and the run continues. |

## The docs trail

Each piece of non-trivial work gets a folder:

```
docs/work/2026-11-04-<topic>/
  brainstorm.md   request, why, decisions (yours vs Claude's), approaches, spikes, outcome
  design.md       projects only: goals, constraints, interfaces, test seams
  plan.md         tasks with live status, results, deviations, and a final Outcome
  roadmap.md      only when the work splits into sub-projects
```

The plan is updated as work happens: steps are ticked, each task gets a status (`done`, `partial`, `blocked`, `skipped`) and a result (commits, test command and output, deviations, review findings). The Outcome records what was done, what wasn't and why, and what's left. After a context reset, work resumes from the plan file, not from memory.

## Credits

lite-powers is derived from two MIT-licensed projects:

- [superpowers](https://github.com/obra/superpowers) by Jesse Vincent: the brainstorm, plan, execute, subagent-driven development, TDD and code-review skills.
- [mattpocock/skills](https://github.com/mattpocock/skills) by Matt Pocock: question rounds (grilling), seams, vertical slices, the debugging loop, domain modeling, handoff, research, and the writing-for-agents principles used to trim everything.

## License

MIT. See [LICENSE](LICENSE).
