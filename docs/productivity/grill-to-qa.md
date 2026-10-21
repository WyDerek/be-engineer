## What it does

`grill-to-qa` runs an interview about an idea, design, or plan, but writes each round of questions to a Markdown file instead of putting them into the chat. You do not have to answer immediately in the conversation, because the questions live on disk. Each round is appended to `qa/<YYYY-MM-DD>-<slug>.md` at the project root, and the [agent](https://www.aihero.dev/ai-coding-dictionary/agent) pauses until you tell it you are done.

It is stateful on disk but leaves the chat clean. The only thing posted to the chat each round is the file path and how many questions are waiting for you.

## When to reach for it

You invoke this by typing `/grill-to-qa`; the agent won't reach for it on its own.

Reach for it when you want to answer questions later, work outside the chat interface, or keep a durable log of the design choices you worked through. Which interview skill you want depends on where you want the questions to live:

- **In the chat.** Use [grill-me](https://aihero.dev/skills-grill-me) for a chat-only interview that leaves no files behind.
- **In a QA file.** Use `grill-to-qa` when you want each round written to a file under `qa/` that you can answer at your own pace.
- **Against a codebase.** Use [grill-with-docs](https://aihero.dev/skills-grill-with-docs) when you want the interview to inspect your code and record domain decisions in `GLOSSARY.md` and ADRs.

## Prerequisites

A project root with a workspace that can hold the `qa/` directory. The skill creates `qa/<YYYY-MM-DD>-<slug>.md` at the project root on the first round and appends every later round to the end of that file.

## The QA file is the record, not the chat

In a typical [grilling](https://aihero.dev/skills-grilling) [session](https://www.aihero.dev/ai-coding-dictionary/session), questions and answers stream directly into the conversation. If you need time to think, consult documentation, or step away, the chat hangs open waiting for input.

`grill-to-qa` separates question generation from answering. The file is the record, not the chat. When a round is ready:

1. The agent writes the round into `qa/<YYYY-MM-DD>-<slug>.md` with `**Status:** open`.
2. In the chat, it prints only the file path and the number of questions waiting.
3. You open the file in your editor and answer each question with an option letter, or your own text when no option fits. Then reply in chat when you are finished.
4. The agent re-reads the file, marks the status answered, and calculates the next frontier.
5. When you confirm the shared understanding, the agent marks the file closed and adds a `## Decisions` summary at the end, one line per question.

The history of questions, options, recommendations, and your answers stays documented in the repository rather than lost in conversation history.

## Common questions

**How do I signal I have answered?**
Tell the agent in the chat when you are done. Say "done", "answered", or "ready". Any other reply is treated as a side question, and the agent keeps waiting. The agent reads the QA file, marks the round answered, and presents the next round or closes the session if no questions remain.

**Why not just use grill-me?**
Use [grill-me](https://aihero.dev/skills-grill-me) when you want a fast, interactive session directly in the chat and do not need a file left behind. Reach for `grill-to-qa` when you want to take your time answering, edit your thoughts in a text editor, or keep a committed record of why decisions were made.

## It's working if

- A file appears under `qa/` on the first round, named with today's date and the topic slug.
- The chat message reports only the file path and question count, without printing the question text in the chat.
- The agent halts after writing each round and waits for your cue instead of prompting you again.
- When you notify the agent you are done, it reads your answers from the file and sets the status to answered.
- Follow-up rounds append to the same file with continuing question IDs (Q1, Q2, and onward).
- When the session closes, the file ends with a `## Decisions` list giving one line per question.

## Where it fits

`grill-to-qa` is a reach-for-it-anytime standalone skill. It does not belong to a single step of the build pipeline, so you can use it whenever a decision, spec, or user story needs clarifying.

It neighbours [grill-me](https://aihero.dev/skills-grill-me) and [grilling](https://aihero.dev/skills-grilling). All three share the same interview logic, but `grill-me` is chat-only and stateless, while `grill-to-qa` writes each round to disk so you can answer outside the chat. When you are unsure which flow fits your situation, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
