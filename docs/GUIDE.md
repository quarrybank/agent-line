# The Agent Line guide

How to run Claude as a small dev team instead of one very long chat. Written for both humans and Claude: hand this file to your Claude and it will understand the system you're building.

---

## 1. Why

One long chat is planner and builder at once. Every new idea becomes a detour, the context fills with half-finished threads, and by hour four nobody remembers the goal. The fix is to split thinking from building, and to keep the truth somewhere that isn't a chat window.

| Before | After |
|---|---|
| Plan, code, debug and test in one thread | A planning chat writes tickets; build agents do one ticket each |
| Tangents happen inside the work | Tangents become `parking-lot` tickets |
| "Done" means it looked fine on screen | "Done" means the checks pass on GitHub |
| Progress lives in your head and chat history | Progress lives in GitHub, readable by anyone or any agent |

---

## 2. The system map

Three lanes. Start with Plan and Build. Add Run when you need it.

### Plan lane

**You, as product owner.** Your job shrinks to three things: set the goal, answer blocking questions up front, press merge. You stop being the one who has to remember everything.

**Planning chat.** A Claude chat that only thinks and writes tickets. It reads and writes GitHub through the GitHub MCP connector, so there's no copy and paste. It doesn't write code. When a tangent shows up, it files an issue instead of chasing it.

**GitHub Issues.** The spec and the state. Every piece of work is an issue with the full brief in its body, so any agent can pick it up cold. Labels show what's ready, blocked or parked.

### Build lane

**Build agent.** A fresh Claude Code session for each ticket. It knows nothing until it reads `CLAUDE.md`, the ticket and the relevant skills, which is why it doesn't drift. One ticket in, one PR out.

**Pull request.** Every change arrives as a PR that opens with a plain-English summary of what changed for users. That's what you read at merge time, maybe days later, after you've forgotten the context.

**CI quality gates.** GitHub Actions runs lint, types and tests on every PR, and branch protection makes them mandatory. The checks decide when something is done, not the agent's opinion of its own work.

**Merge and deploy.** Green PR, you press merge, it deploys. Low-risk changes (docs, tests) can merge themselves. Anything touching the database schema, config or secrets waits for you.

### Run lane (Level 4, when it hurts)

**Scheduled workers** (for example Cloudflare Workers with cron triggers). Tiny functions that run on a schedule or fire on an event. They check things are still moving, route errors, and open a GitHub issue when something breaks, so an agent can pick it up. They keep running when every chat is closed. Good fit for kicking off crawls, checking data freshness, and alerting when a source stops returning data.

**Your own MCP server.** Exposes your app's own operations as tools Claude can call from any chat: look up a record, re-run a job, check a queue. A chat can then act on the real system without you in the middle, and changes made from a chat are stored as proper state rather than lost in the conversation.

**Runner or server.** Where builds and tests run. GitHub's hosted runners are fine to start. A self-hosted runner (a cheap VPS or a box at home) makes sense once paid Actions minutes or slow test suites start to bite. It's a one-off cost rather than a growing monthly bill, and it gives agents a stable place to run a staging copy.

---

## 3. A ticket's journey

Stops marked 🟡 need a human.

1. 🟡 **Plan the week.** One sprint goal the team controls ("crawler for 3 new sites lands clean listings", not "get 1,000 users"). Pick the tickets that serve it.
2. 🟡 **Refine.** Each ticket gets a rough approach and its blockers named. Answer them all now, in one sitting. (`/refine`)
3. **Fire the agent.** A fresh session gets one ticket: "Build #12. Follow CLAUDE.md."
4. **Code and tests.** The definition of done names the test that proves it.
5. **CI decides.** Red means the agent fixes it on the same branch. A failing check is a repair, not a new ticket.
6. 🟡 **Merge.** Read the summary, not the diff. Press merge.
7. **Verify live.** Check the change works where users see it, close the loop, record anything learned. (`/merge`)
8. 🟡 **Retro.** What shipped against the goal? What went wrong twice? Repeat failures become rules or tests.

---

## 4. Fresh agents, loaded the same way

Build agents start blank every time. They can't drift, carry a bad assumption from last week, or get tired. They read the same files at the start of every job instead:

- **`CLAUDE.md`**: house rules. Stack, commands, how to test, folder layout, gotchas.
- **The ticket**: the full spec. User story, definition of done, verify steps, likely files.
- **Skills**: reusable how-tos in `.claude/skills/`.
- **State**: where things stand, on issues and PRs, never only in a chat. `/status` reads it.

Changing how agents behave is a pull request against a text file. When something goes wrong twice, you don't remind Claude. You edit the rule.

---

## 5. Quality gates

If the checks are good, you can trust agent work without reading every line. If they're weak, no amount of prompting saves you.

| Gate | Catches | Who acts |
|---|---|---|
| Lint + type check | Sloppy code, wrong types, broken imports | Agent fixes |
| Unit tests | Logic that used to work and now doesn't | Agent fixes |
| Golden fixtures | Saved copies of real pages your scrapers read, parsed in tests. A layout change fails a test instead of putting bad data in your database. | Agent fixes |
| Schema checks | Records missing required fields. Rejected and logged, never stored. | Agent fixes |
| Real-storage tests | Tests that pass against a fake while the real database fails. Write, read back from a fresh connection, check the value. | Agent fixes |
| Branch protection | Turns all of the above from advice into a rule: nothing reaches `main` red. | Human sets once |
| Merge rule | Docs and tests can auto-merge. Code, config and migrations wait. | Human merges |

---

## 6. Where to start

**Level 1: GitHub is the memory** (an afternoon)
- Connect the GitHub MCP connector to Claude.
- Write `CLAUDE.md`.
- Move your to-do list into issues with the Story template.

**Level 2: Tests decide** (a day)
- CI on every PR: lint, types, tests.
- Golden fixtures for anything you scrape.
- Branch protection on `main`.

**Level 3: Planner ≠ builder** (a week)
- One planning chat that only writes and refines tickets.
- Builds run as separate Claude Code sessions (cloud sessions or the Claude Code GitHub Action), one ticket and one PR each.
- A weekly sprint goal and a Friday retro.

**Level 4: Things run on their own** (when it hurts)
- Scheduled workers for crawls, freshness checks and failure alerts that open issues.
- Your own MCP server.
- A self-hosted runner.

---

## 7. Focus rules

- **Ideas become tickets.** A new idea mid-build goes into a `parking-lot` issue in thirty seconds. Then back to the ticket you were on.
- **One ticket, one behaviour.** Small enough to test, big enough to matter.
- **Finish before starting.** One build in flight to begin with. Unmerged work goes stale fast, and parallel agents collide on the same files.
- **Decide early, in a batch.** Questions for the human come at refinement, not drip-fed while they're trying to focus.
- **Cap every session.** Time and cost limits on agent runs. Hours in means lost, not hard-working.
- **Ask the north-star question.** Does this week move the product toward what you actually want? If not, it waits.

---

## 8. The skills

| Skill | Use it when |
|---|---|
| `/ticket` | An idea, bug or feature needs to become a buildable issue, or a tangent needs parking |
| `/bug` | Something broke. Diagnoses the class of bug (infrastructure or code, source changed, false-green test) before anyone writes a fix |
| `/refine` | Before a sprint. A panel of specialist subagents sizes and splits tickets; all human questions come back in one list |
| `/status` | Start of a work block. Reads GitHub and the live app, reports what's merged but unverified and what's blocked on you |
| `/merge` | After every merge. Closes the ticket, records gotchas in `CLAUDE.md`, verifies the change live |
| `/roundtable` | A decision spanning product, cost, security and engineering |
| `/reverse-uno` | You've patched the same thing three times, or face a hard-to-undo choice |
