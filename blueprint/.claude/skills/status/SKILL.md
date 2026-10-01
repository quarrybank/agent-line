---
name: status
description: "Read the true current state of the project from GitHub and the live app, never from memory, and report it on one screen - sprint goal, in flight, merged but unverified, verified, blocked on the human, next three moves. Use at the start of a work block, after time away, after a batch of merges, or when the user asks where are we, status, recap, or pick up where we left off."
---

# /status

One screen, read from live sources. It stops you re-deriving state by memory, and stops "merged" being mistaken for "working".

## Read in this order
1. **Open PRs:** title, checks, age. An old PR with red checks is a problem, not just a row. A PR stuck behind an out-of-date branch should be updated, not reported.
2. **Open issues:** anything labelled `blocked`, and what's `ready` for the sprint goal.
3. **Merges since the last status:** for each, has anyone checked it works in the deployed app?
4. **Live checks:** for anything claimed done but not proven, the cheapest real check (load the page, query the data, read the job log). A green test in the PR is not proof the deployed app changed.
5. **Background jobs:** did the scheduled crawls run? When did each source last return fresh listings?
6. **Human blockers:** keys, quotas, billing, settings. These don't move without a person.

## Report
```
SPRINT GOAL         one line, on track or not
IN FLIGHT           issue -> PR -> state
MERGED, UNVERIFIED  the dangerous column. Empty is the goal.
VERIFIED            what was proven, and how
DATA HEALTH         each source: last good crawl, record count trend
BLOCKED ON YOU      each with the exact step and a direct link
NEXT THREE          in order
```

## Rules
- A merge is not a fix. Done means seen working where users see it.
- Wait for the deploy to finish before checking, or you'll read the old version.
- A check that never reports (missing or misconfigured) is not "still running". Look for it when a PR sits pending forever.
- Three failures in a row are one cause, not three bad sessions.
