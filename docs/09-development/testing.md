# Testing

| Field | Value |
|---|---|
| Status | `CURRENT` inventory; coverage and CI are `OPEN` |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Frontend/package.json`](../../../Just_Cook_Frontend/package.json); [`Just_Cook_Frontend/cypress.config.ts`](../../../Just_Cook_Frontend/cypress.config.ts); [`Just_Cook_Backend/src/api/tests/test_auth.py`](../../../Just_Cook_Backend/src/api/tests/test_auth.py); repository trees reviewed on 2026-09-02 |

## Purpose

This page records test commands and test files that were visible in the source
snapshot. No application test, build, or end-to-end run was performed while
creating this documentation.

## Frontend test commands

The frontend package manifest defines:

| Command | Intended scope |
|---|---|
| `npm run test:unit` | Vitest unit tests |
| `npm run test:e2e` | Cypress end-to-end tests |
| `npm run lint` | ESLint |
| `npm run build` | Vite production build, not a test |

The Cypress configuration uses `http://localhost:5173` as its base URL and
expects specs below `tests/e2e/specs/**/*.cy.{js,jsx,ts,tsx}`. No matching spec
files were visible in the reviewed tree. The existence of a script therefore
does not establish end-to-end coverage.

## Backend test coverage visible

The visible Backend test file is
[`src/api/tests/test_auth.py`](../../../Just_Cook_Backend/src/api/tests/test_auth.py).
It uses Django `SimpleTestCase`, `RequestFactory`, and a mocked token validator
to check:

- a missing Authorization header returns `401` and an error message;
- a verified `sub` claim is made available as the request user ID.

The inspected tree did not establish complete tests for recipe CRUD, category
ownership, sharing, OCR, website import, migrations, or external integrations.

## Auth and Index

No visible Auth test suite, CI workflow, or test command was established in the
reviewed Auth repository. The Index repository likewise has no visible tests.

## Recommended test layers

These are `PLANNED` recommendations, not claims about existing coverage:

1. Unit-test recipe-body and category transformations on both sides of the API.
2. Add Backend tests for ownership and missing-resource behavior for every CRUD
   endpoint.
3. Add contract tests for recipe list/detail fields and AI response envelopes.
4. Test Auth-to-Backend JWT validation with fixed algorithm, issuer, audience,
   expiry, and key rotation cases.
5. Add browser tests for registration verification, logout cache clearing, and
   public sharing.
6. Add deployment smoke checks for health, static routes, CORS, JWKS, and image
   paths.

## Open test decisions

- Which test command and dependency source is authoritative for each repository?
- Which databases and external providers are replaced by test doubles?
- What CI, coverage, and release gates are required?
- Which environments may run browser tests against shared services?

## Further reading

- [Coding guidelines](coding-guidelines.md)
- [Authentication flow](../03-auth/authentication-flow.md)
- [API overview](../07-api/api-overview.md)
- [Deployment](../08-deployment/deployment.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial static test inventory and planned test layers |
