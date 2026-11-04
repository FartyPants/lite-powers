---
name: domain-model
description: Use when resolving what a project term means or recording a hard-to-reverse design decision - writes GLOSSARY.md terms and ADRs in docs/adr/.
---

# Domain Model

Build and sharpen the project's shared language while designing. Reading `GLOSSARY.md` for vocabulary is a habit every skill has; this skill is for *changing* the model.

## During the conversation

- **Challenge against the glossary.** A term used differently from `GLOSSARY.md` gets called out at once: "Your glossary defines 'cancellation' as X, but you seem to mean Y. Which is it?"
- **Sharpen fuzzy language.** For a vague or overloaded word, propose one precise term: "'account' - the Customer or the User?"
- **Probe with scenarios.** Invent concrete edge cases that force precision about the boundaries between concepts.
- **Cross-check the code.** When the user says how something works, check that the code agrees, and surface contradictions.

## GLOSSARY.md

Write each term into `GLOSSARY.md` (repo root) the moment it's resolved. Create the file with the first term. If a `GLOSSARY-MAP.md` exists, use the context-specific glossary it points to.

```md
# <Context name>
<1-2 sentences: what this context is.>

## Language
**Order**: A customer's request to buy one or more items.
_Avoid_: purchase, transaction
```

- One or two sentences per term, saying what it *is*.
- Pick one word per concept and list the rejected synonyms under `_Avoid_`.
- Only project-specific concepts: domain terms, not general programming terms and not implementation details.
- Group terms under subheadings when natural clusters appear.

## ADRs

Offer an ADR only when all three hold: the decision is **hard to reverse**, **surprising without context**, and the result of a **real trade-off**. Examples: architectural shape, integration patterns, lock-in technology, ownership boundaries, deliberate deviations from the obvious path, constraints the code can't show, non-obvious rejected alternatives.

File: `docs/adr/NNNN-<slug>.md`, numbered one above the highest existing number (create the folder with the first ADR):

```md
# <Short decision title>
<1-3 sentences: the context, what we decided, and why.>
```

Add Status (`proposed | accepted | superseded by NNNN`), Considered options or Consequences only when they carry real value.

Link new ADRs from the topic's design.md when one exists.
