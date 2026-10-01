# Installing Agent Line

Instructions for Claude Code. A human can follow them too.

## If you're in someone's existing repo

1. Clone `https://github.com/quarrybank/agent-line` to a temporary folder.
2. Create a branch `agent-line-setup`.
3. Copy everything inside `blueprint/` into the repo root. **Never overwrite an existing file.** If a file already exists (`CLAUDE.md`, a PR template, a CI workflow), merge the Agent Line content into it and keep everything that's already there.
4. Fill in every `TODO` in `CLAUDE.md` by reading the codebase: stack, commands, layout. Ask the user only about what you can't infer, all questions in one batch.
5. Adapt `.github/workflows/ci.yml` to the project's real lint, type check and test commands. If the project already has CI, add any missing checks to it instead of creating a second workflow.
6. If the project has crawlers or scrapers, check whether `tests/fixtures/` exists. If not, list the sources and propose (don't build) a ticket per source for fixture-based parser tests.
7. Commit, push the branch, and open a PR whose body starts with a user story and lists the steps the human must do themselves (below).
8. Stop. Don't merge.

## If you're in a fresh repo made from the template

Do the same, but move `blueprint/` contents to the root, then delete `blueprint/`, `INSTALL.md`, `docs/GUIDE.md` and the Agent Line `README.md` (replace it with a short README for the new project).

## Steps only the human can do

Tell the user these, each with a direct link built from the repo's owner and name:

- **Protect `main`:** `https://github.com/<owner>/<repo>/settings/branches`. Add a rule for `main`: require a pull request, and require the CI checks to pass.
- **Connect GitHub to Claude** (if not already): add the GitHub connector in Claude's settings so the planning chat can read and write issues.
- **Create labels:** `ready`, `blocked`, `parking-lot`. You can do this yourself with the GitHub CLI or API if you have access. Only ask the user if you can't.
