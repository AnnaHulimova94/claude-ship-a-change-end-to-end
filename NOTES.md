# NOTES

## Plan
I approved a three-step plan: add `updateUser(id, { name, email })` to `db/store.js`
(returns `undefined` when the user doesn't exist), add `PUT /users/:id` to `routes/users.js`,
and write these notes. The route checks in this order: validation first (400), then lookup
(404). Validation is stricter than in POST: `name` and `email` must be non-empty strings.
I didn't edit the plan before approving it.

## Model
I chose Opus: it is more reliable on changes where edge cases matter (400/404 ordering,
invalid types in the request body). The task is small, but a mistake in validation logic
costs more than the speed difference saves.

## Commits
Three logical commits: (1) the store helper, (2) the route with validation and 404,
(3) `NOTES.md`. The store and the route are separate so each commit has a single reason to
exist and can be understood or reverted on its own. The update-user tests are green after
commit 2.

## Review
The review confirmed the main paths are fine: `express.json()` yields `{}` for an empty body
(so we return 400, not a crash), a non-numeric id becomes `NaN` and returns 404, and
malformed JSON is handled by Express (400). Findings I deliberately left alone: values are
not trimmed before saving (same as POST), email format is not validated (out of scope), and
`updateUser` mutates the shared in-memory object (same as the rest of the store's pattern).
