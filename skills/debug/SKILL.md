---
name: debug
description: Use when something is broken, throwing, failing, flaky or slow and the cause isn't obvious - build a feedback loop that reproduces it before proposing any fix.
---

# Debug

If you have a **tight** pass/fail signal that goes **red** on this exact bug, you'll find the cause; everything else just consumes that signal. So build the loop first.

Redact secrets in every command and output you show (`<REDACTED>`); keep credentials in env vars.

## 1. Build the loop

Build one command that reproduces the user's exact symptom. Try these in roughly this order:
1. a failing test at a seam that reaches the bug
2. a curl/HTTP script against a running dev server
3. a CLI run with a fixture input, diffed against a known-good output
4. a headless browser script asserting on DOM, console or network
5. a replay of a captured request, payload or log
6. a throwaway harness around the one code path
7. a property/fuzz loop for "sometimes wrong" bugs
8. `git bisect run` between a known-good and a known-bad state
9. a differential run comparing old and new versions or configs

Then tighten it: make it faster, assert on the specific symptom, and make it deterministic (pin time, seed RNG, isolate filesystem and network). For flaky bugs, push the reproduction rate up with loops, parallelism and stress until it's debuggable.

**Done when** you've run one command, shown its output, and it is red-capable (it asserts the user's symptom), deterministic, fast (seconds), and runnable without a human. If you can't build one, say what you tried and ask for access, a captured artifact, or permission to add temporary instrumentation. Theories wait until the loop exists.

## 2. Reproduce and minimise

Confirm the loop shows the user's failure, not a nearby one. Then cut inputs, callers, config and steps one at a time, rerunning after each cut. Stop when every remaining element is load-bearing.

## 3. Hypothesise

List 3-5 ranked, falsifiable hypotheses: "If X is the cause, changing Y makes it disappear." Show the list to the user (they often re-rank instantly) and carry on with your ranking if they're away.

## 4. Instrument

Make one probe per prediction and change one variable at a time. Prefer a debugger or REPL, then targeted logs at boundaries. Tag each debug log with a unique prefix (`[DEBUG-a4f2]`) so cleanup is a single grep. For performance bugs, measure a baseline first, then bisect.

## 5. Fix with a regression test

Turn the minimal repro into a failing test at a seam that exercises the real call pattern. Watch it fail, fix, watch it pass, then rerun the original loop. If no correct seam exists, record that as an architecture finding.

## Inside a plan run

When called from `lite-powers:execute-plan` or `lite-powers:subagent-dev`:
- Carry on with your own hypothesis ranking instead of waiting for the user, and record the confirmed cause in the task's Result.
- In Plain mode, add a regression test where a correct seam exists. Watching it fail first is a TDD-mode requirement.

## Done when

- The original loop is green.
- The regression test passes, or the missing seam is recorded.
- A grep for the `[DEBUG-` prefix finds nothing.
- Spikes are removed.
- The confirmed cause is stated in the commit message (and in plan.md's Result when working from a plan).
