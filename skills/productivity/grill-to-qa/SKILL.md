---
name: grill-to-qa
description: A grilling session whose questions live in a QA file instead of the chat.
disable-model-invocation: true
---

Call the Skill tool with "grilling", with one change: each round goes to a QA file, not the chat.

## The QA file

Write to `qa/<YYYY-MM-DD>-<slug>.md` at the project root. Slug from the topic. Create the file on the first round and add every later round to the end of the same file.

````markdown
# QA: <topic>

**Stage:** us | spec | ticket | bug | clarify
**Chain:** [US-12](../docs/us/12-notify.md) → [spec](../.scratch/mail/spec.md)
**Started:** <YYYY-MM-DD>
**Status:** open

## Questions for You

### Q1 — <question title>

**Refs:** [spec § Failure handling](../.scratch/mail/spec.md#failure-handling)

- **A (recommended):** ...
- **B:** ...

**Your answer:**
````

- `Stage` is the stage of the flow the grilling sits in. `Chain` links up the flow to the story, spec or ticket this one came from. Leave `Chain` out when there is nothing to link.
- `Refs` is optional: links to what the user may need to read before answering.
- Exactly one option is recommended. An open question with no options is fine: put your recommended answer in the body.
- Ids keep counting across rounds (Q1, Q2, ...).

## The loop

1. Write the round to the file with `**Status:** open`. In the chat, say only the path and how many questions are waiting. Do not repeat the questions there.
2. Stop and wait. The user answers in the file, then tells you they are done.
3. Re-read the whole file, set `**Status:** answered`, and recompute the frontier from the answers.
4. If more questions remain, add the next round and set `**Status:** open` again.
5. When the user confirms the shared understanding, set `**Status:** closed`.
