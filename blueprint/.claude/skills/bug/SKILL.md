---
name: bug
description: "Diagnose a bug before anyone writes a fix - get the real error, check the common bug classes, decide whether it's infrastructure or code, then hand back either the exact human step or a ready ticket with the test that would have caught it. Use when the user says /bug, this is broken, not working, it failed, pastes an error, or a crawler stops returning data."
---

# /bug

Diagnose first. A fix aimed at the wrong cause burns a session and often ships a test that passes while the real problem stays.

## 1. Get the real error
A generic message ("Something went wrong", "Crawl failed") is not the error. Find the real one: the exception, the HTTP status and body, the log line, the exact value that was wrong. If the code hides the real error behind a generic one, that is the first bug to fix.

## 2. Check the common classes, in order
For each, say what evidence points to it. Don't guess.

1. **Infrastructure, not code.** Missing API key or env var, expired token, quota or rate limit hit, permissions, DNS, billing. Fix is a human step, not a session.
2. **Source site changed.** A crawler returns empty or malformed records because the page layout changed. Fix: save a fresh copy of the page as a fixture, update the parser, test against it.
3. **Blocked or throttled by the source.** 403s, captchas, 429s. Back off; it's not a parser bug.
4. **Bad data got through.** Records with missing or wrong fields reached the database. Fix the parser and tighten the schema check so the same shape is rejected next time.
5. **False-green test.** Tests pass against a fake or mock while the real database or API fails. Fix: a test against real storage that writes, reads back from a fresh connection, and checks the value.
6. **Migration on old data.** Works on a fresh database in CI, fails in production because the existing table has the old shape. Fix: make the migration safe to run on existing data, and test from the old shape.
7. **Not actually deployed.** The fix merged but the old code is still running (cache, failed deploy). Check the deploy run and test the behaviour, not a version number.
8. **Genuinely new logic bug.** None of the above. Normal ticket.

## 3. Output
```
## Bug: <one-line symptom>
Real error: <the actual message>
Class: <number and name> - evidence: <why>
Type: infrastructure | code

Next step:
<either the exact human action, with a direct link>
<or a ticket via /ticket, with this definition of done:>
  - The real error is surfaced, not swallowed
  - A test reproduces this failure class and fails without the fix
  - CI green; PR opened; not merged
```

## Rules
- Never send a coding session at an infrastructure problem.
- Three failures of the same kind in a row means a shared cause. Stop and find it before trying a fourth fix (consider /reverse-uno).
