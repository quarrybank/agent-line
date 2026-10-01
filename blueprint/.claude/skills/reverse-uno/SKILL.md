---
name: reverse-uno
description: "Step back and challenge the foundation before building more on it - name the assumption everyone has taken for granted, steelman the alternatives, pick a phased direction that starts with a cheap reversible step, then pressure-test it with a roundtable. Use when the same thing has been patched three or more times, before a hard-to-undo choice (data model, hosting, database, scraping approach, the platform you're on), or when the user asks whether X is even the right tool."
---

# /reverse-uno

Instead of taking the next turn in the same direction (another patch, another layer), reverse it and ask whether the direction itself is wrong.

## When
- The same bug or wall has been hit three or more times.
- A hard-to-undo choice is being made without anyone examining it.
- Someone asks "is X even the right place for this?"
- A lot of time has gone into one kind of problem and progress has stalled.

## Method
1. **Name the lock-in.** The foundation in question, what triggered this, and the assumption everyone has been building on ("we store everything in X", "we scrape with Y"). One paragraph. If you can't name the assumption, you haven't found it.
2. **Reverse the default.** Argue honestly for and against it. What is it actually costing? Would the problem disappear under a different foundation? Separate what's right (keep) from what's the real limit (change).
3. **Real alternatives.** Two to four options, typically: keep and patch, a moderate change, a bigger change. For each: cost, risk, effort, and what it unblocks. Treat doing nothing fairly.
4. **Pick a phased direction.** A recommendation with a one-line reason, starting with a cheap, reversible first step that proves the idea. Ideally that first step also fixes the current pain.
5. **Pressure-test it** with /roundtable. Let the panel change the answer if they're right.
6. **Land it.** A short decision record, the backlog updated via /ticket, and the single next step.

## Quality bar
- The reversal is real, not a token gesture before rubber-stamping the status quo.
- It ends in a decision, tickets and one next step. A discussion alone is a failure.
