---
name: roundtable
description: "Run a short cross-functional expert meeting (product, design, growth, finance, security, red team, engineering, a domain expert) to pressure-test a decision, surface real trade-offs and disagreement, and end with explicit decisions folded into the backlog. Use when the user asks for a roundtable, panel, or the team's take, or for any decision that spans disciplines or is hard to undo (data model, pricing, auth, hosting, paid APIs)."
---

# /roundtable

Turn an open question into a decided plan by running a realistic meeting.

## Cast
Founder/product owner (chairs), Design, Growth, Finance, Security, Red team, Backend, Frontend, plus a domain expert (for a co-living app: someone who has run or lived in co-living spaces). Add or drop people to fit. Give them names so it reads like people.

## Method
1. Frame the agenda as the actual open decisions, not "improve X".
2. Each person speaks from real expertise and self-interest, specific to this product: real numbers, named risks, real tools. No platitudes.
3. Surface genuine disagreement (speed vs safety, cost vs features, scope vs polish). Don't manufacture consensus.
4. Security and red team raise concrete attack or failure paths: scraped content containing instructions an AI might follow, a paid API being hammered, secrets leaking, one user seeing another's data.
5. End each agenda item with an explicit **DECISION** and a one-line reason.
6. Contributions stay to 2 to 4 sentences. Someone with nothing real to add stays quiet.

## Output
- The transcript, grouped by agenda item, each ending in a DECISION.
- A "Resolved decisions" summary.
- The decisions folded into the backlog: tickets filed or updated via /ticket, and the one next step.

If nothing was decided and nothing changed in the backlog, it didn't work.
