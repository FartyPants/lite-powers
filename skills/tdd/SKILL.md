---
name: tdd
description: Use ONLY when the user has chosen TDD as the testing mode this session, or explicitly asks for test-driven development / red-green-refactor.
---

# TDD

Red, then green, in **vertical slices**: one test, one minimal implementation, repeat. A test you never watched fail proves nothing.

## Seams first

A **seam** is a public boundary where you observe behavior without reaching inside. Tests live at seams only.

Before the first test, list the seams under test, each with one line on what it catches and what it misses. Use the design's "Testing decisions / seams" or the plan's **Tests at:** if they exist; otherwise confirm the seams with the user. Prefer existing seams, the highest one that reaches the behavior, and as few as possible.

If `GLOSSARY.md` exists, name tests in its vocabulary.

## The loop

1. **RED**: write one test for one behavior at an agreed seam, with a name that reads like a spec ("rejects empty email").
2. **Verify RED**: run it. It must *fail*, for the expected reason (the behavior is missing).
   - Already passes: it's testing existing behavior, so change the test.
   - Errors: fix the error and rerun until it fails properly.
3. **GREEN**: write the simplest code that passes.
4. **Verify GREEN**: run it, then the full suite. Everything should be green with clean output. If it fails, fix the code and keep the test.
5. **REFACTOR**: improve names and remove duplication while staying green.
6. Next slice: let what this cycle taught you shape the next test.

Code written before its test gets set aside, and the slice restarts from the test.

## Tests worth keeping

- Assert on behavior through the public interface, so the test survives a refactor of the internals.
- Take expected values from an independent source: a known-good literal, a worked example, the spec. An assertion that recomputes the value the way the code does (**tautological**) passes by construction.
- Mock only at system boundaries (network, clock, filesystem), never internal collaborators.
- Write tests one slice at a time. Writing them all up front (**horizontal slicing**) tests imagined behavior.
- Hard to test usually means hard to use. Simplify the interface.

## Bugs

Reproduce the bug as a failing test at a seam that exercises the real call pattern, then fix it. If no such seam exists, record that as a finding.

Throwaway spikes, generated code and pure config are exempt; say so when you skip TDD.

## Done when

- Every new behavior has a test at an agreed seam that you watched fail for the right reason.
- The full suite passes with clean output.
