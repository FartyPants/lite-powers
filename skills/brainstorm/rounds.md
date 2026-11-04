# Question rounds

Shared by `lite-powers:brainstorm`, `/grill` and `/grill-docs`.

Map the topic as a **design tree**: each decision branches into the decisions that hang off it. The **frontier** is every decision whose prerequisites are already settled.

Each round, ask the whole frontier as plain text, numbered, each question with your recommended answer. Word each question so that "yes" accepts your recommendation:

```
**Q1 - <title>**: <question; may include options>
-> Recommended: <answer>

**Q2 - ...**
```

Then wait for the answers. A question that depends on an answer still open this round belongs to a later round. After each round, recompute the frontier: settled decisions unblock the questions that hung on them.

**Facts are your job; decisions are theirs.** Look up anything the codebase, files or web can answer: yourself, or with an Explore subagent running while the rest of the frontier is asked (only the questions downstream of it wait). Put only decisions to the user, and let the user answer them; never answer for them.

The rounds are done when the frontier is empty: every branch visited, nothing silently assumed. Confirm the shared understanding with the user before acting on it.
