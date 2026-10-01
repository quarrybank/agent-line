---
name: refine
description: "Run sprint refinement on the next batch of tickets - a small panel of specialist subagents sizes, splits and flags blockers independently, then you verify their claims and rewrite the tickets so they're buildable. Ends with every human question batched in one list. Use when the user says /refine, refinement, sprint planning, or before starting a week's work."
---

# /refine

The aim: all the questions that need the human get asked now, in one sitting, so the week's builds run without interruptions.

## 0. Close last sprint's loop
List PRs merged since the last refinement. From their review comments, pull every concern or warning that wasn't acted on. For each: file a ticket, or dismiss it with a one-line reason. Keep the list in one issue comment so nothing is lost.

## 1. Pick the tickets
The next 5 to 10 tickets that serve this sprint's goal. If there is no written sprint goal, ask for one first. It must be something the team controls ("crawler for 3 new sites lands clean listings"), not an outcome it doesn't (traffic, signups).

## 2. Convene the panel
Spawn these in parallel as read-only subagents. Each gets a different background to read, because the same context gives the same opinion several times.

- **Engineer:** CLAUDE.md, the code layout, recent merged PRs in the areas touched.
- **QA:** docs/DEFINITION_OF_DONE.md, the tests folder, known bug classes. Rewrites acceptance criteria so they're checkable by a machine.
- **Product:** the live app and the sprint goal. Is the story written from the user's side? Does it serve the goal?
- **Risk:** secrets, scraping terms and rate limits, personal data, cost of any paid API.

Each returns, per ticket, in under 120 words: relevance 0-10, size estimate (small / medium / large, independently, without seeing the others), concerns, suggested split, blockers, questions for the human.

## 3. Decide
- Only voices scoring 6+ on relevance count for that ticket.
- Estimates differ a lot? Split the ticket. Smaller is the cheaper way to be wrong.
- Majority says split: split.

## 4. Verify before you write
Panel output is model output. Before it changes a ticket, check each claim against the code or git history. A "bug" might be deliberate. A suggested fix might be wrong.

## 5. Rewrite the tickets
Each opens with the user story and value line, then Why / Build / Definition of done / Verify / Files / Blockers. Fold any decisions from comments into the body, so the body alone is the spec. Label `ready` or `blocked`.

## 6. Report to the human
Short:
- what changed (splits, risks found);
- **all questions in one numbered list**, each with what it blocks;
- the order to build in.
