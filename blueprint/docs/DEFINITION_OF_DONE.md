# Definition of Done

Every PR is judged against this, whether by you or by a reviewer agent.

1. **Scope.** The change does what its issue asked, and no more. Unrelated fixes belong in their own issue.
2. **Tests.** A behaviour change carries a test that fails when the change is reverted. All CI checks are green.
3. **Real behaviour.** The test checks what a user or caller actually sees (the right data, the right response), not just a 200 status or "no error thrown".
4. **No misleading states.** No fake success messages, placeholder data presented as real, or errors swallowed into a generic message. If something fails, the real error is visible somewhere.
5. **Verified.** The PR body says how it was checked, and where possible what was seen running for real (a URL, a response, a row in the database).
6. **Docs.** If the change alters how something is called or configured, `CLAUDE.md` or the relevant doc says so.
7. **Reversible.** Reverting the commit undoes the change. Anything that can't be undone that way (a migration, a data backfill, a deleted table) is called out at the top of the PR.
8. **Human steps named.** Anything a session can't do itself (secrets, settings, workflow files, billing) is prepared and listed in the PR as a clear step for the human.

Humans merge. Always.
