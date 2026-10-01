---
name: merge
description: "Post-merge bookkeeping - read the merged PR, close its ticket, record any new gotcha or rule in CLAUDE.md, file follow-ups the PR flagged, and check the change works once deployed. Use when the user says /merge, I merged, merged #N, or that's in."
---

# /merge

Keeps the repo's memory current so the next fresh session knows what this one learned.

## Steps
1. **Find the PR.** Use the number given, or the most recent merge on `main`.
2. **Read it:** the summary, what changed, the files, any review comments.
3. **Close the loop.** Confirm the linked issue closed. If the PR flagged follow-ups or a reviewer raised a concern that wasn't fixed, file each with /ticket (or note why it's dismissed).
4. **Record what was learned.** If the work revealed a gotcha, a pattern, or a rule ("Site X needs a 2s delay between pages"), add one terse line to CLAUDE.md under Gotchas, in a small follow-up PR. Remove anything this merge made untrue.
5. **Verify once deployed.** Wait for the deploy to finish, then run the issue's Verify step for real. Report what you saw.
6. **Report in 2 to 4 lines:** what merged, what it changed for users, verified or not, what's next.

## Rules
- CLAUDE.md stays short. Facts that change behaviour, not a changelog.
- Never write secrets or keys anywhere.
- If verification fails, say so plainly and run /bug. Don't reopen the old PR.
