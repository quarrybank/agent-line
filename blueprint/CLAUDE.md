# CLAUDE.md

Every Claude session in this repo reads this file first. Keep it short, blunt and current. When something goes wrong twice, add a line here.

## What this is
TODO: one paragraph. What the product does, who uses it, what matters most.

## Stack
TODO: language, framework, database, hosting, where crawlers/jobs run.

## Commands
```
install:    TODO
dev server: TODO
lint:       TODO
typecheck:  TODO
test:       TODO
all checks: TODO   # the one command CI runs
```

## Layout
TODO: the 5 to 10 folders that matter and what lives in each.

## How we work
1. **No PR without an issue.** Work from the issue you were given. If none exists, stop and ask for one.
2. **One issue, one branch, one PR.** Branch name: `issue-<n>-short-slug`. PR body includes `Closes #<n>`.
3. **The issue is the spec.** If it's ambiguous, ask in one batch before starting, not halfway through.
4. **Stay in scope.** Found something else broken? File a new issue for it and keep going. Don't fix it in this PR.
5. **A failing check is a repair, not a new ticket.** Fix it on the same branch and PR.
6. **Never push to `main`. Never merge.** Open the PR, make the checks green, stop.
7. **Don't touch** `.github/workflows/`, secrets, or database migrations unless the issue says to. If you need one of those changed, prepare it and say what the human needs to do.
8. **Done means** everything in `docs/DEFINITION_OF_DONE.md`.

## Testing rules
- A behaviour change ships with a test that fails if you revert the change.
- Test against real storage where it matters. A test that passes against an in-memory fake while the real database fails is worse than no test.
- Crawlers: every source has saved sample pages in `tests/fixtures/<source>/` and parser tests against them. When a site changes its layout, update the fixture and the parser together.
- Scraped records go through the schema check before they're stored. Rejected rows are logged with the reason, never silently dropped or silently stored.

## Gotchas
TODO: add a line every time something bites. Examples of the shape:
- `<thing>` looks saved but isn't until `<step>`; always read back from a fresh connection.
- `<source site>` rate-limits after N requests a minute; the crawler backs off, don't remove it.
