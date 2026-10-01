---
name: ticket
description: "Turn an idea, bug or feature into a GitHub issue that a fresh Claude session can build from alone - user story, why, build, definition of done, verify, files. Splits big work into a parent story plus child tickets. Use when the user says /ticket, file this, make a ticket, write this up, or mentions a new idea mid-task (file it as parking-lot and return to the current work)."
---

# /ticket

Write work down so that any fresh session can pick it up cold. The issue body is the spec. Never point at a file the session can't see ("see my notes.md").

## Steps

1. **Check for a duplicate.** Search open issues for the same thing. If one exists, add to it instead.
2. **Size it.** One ticket = one behaviour plus its tests. If the summary needs "and also", it's two tickets. If it touches several areas, make a parent story and one child per area.
3. **Write the body** in this order:
   ```
   ## User story
   As a <who>, I want <what>, so that <why>.
   **Value:** <one plain-English line>

   ## Why
   ## Build
   ## Definition of done      (name the test that proves it)
   ## Verify                  (URL / query / command and what it should return)
   ## Files                   (likely paths, one per line)
   ## Blockers                (blocked: need X to confirm Y)
   ```
4. **Parent and children.** The parent holds the full spec and a `## Tickets` checklist of the children. Each child says `Part of #<parent>`, states its own slice, and carries its own Definition of done and Verify.
5. **Label it.** `ready` if it can be built now, `blocked` if it needs a human first, `parking-lot` if it's a new idea that came up during other work.
6. **Report** the issue link(s). Never bare numbers.

## Tangents

When the user has an idea while something else is in progress, file it as `parking-lot` in under a minute with whatever detail exists, say "Filed as #N for planning", and go back to the current work. Do not start on it.

## Rules
- Anything that's a human judgement (prices, legal wording, which provider to pay for) goes in Blockers, not left for a session to guess.
- A ticket without a Definition of done and a Verify section isn't ready.
