# Coding Guidelines

| Field | Value |
|---|---|
| Status | `CURRENT` observed conventions; broader standards are `OPEN` |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Frontend/.eslintrc.cjs`](../../../Just_Cook_Frontend/.eslintrc.cjs); [`Just_Cook_Frontend/src/public/ts/requests/`](../../../Just_Cook_Frontend/src/public/ts/requests); [`Just_Cook_Backend/src/api/`](../../../Just_Cook_Backend/src/api); [`Just_Cook_Auth/src/`](../../../Just_Cook_Auth/src) |

## Purpose

These are lightweight rules inferred from the current code. They aim to keep
cross-service changes understandable without introducing a new style system.

## Cross-service rules

- Keep user-facing behavior and API contracts synchronized across Frontend and
  Backend.
- Link technical documentation to source files rather than copying large code
  blocks.
- Never place secrets, tokens, private keys, or real credentials in source,
  examples, logs, or tests.
- Label unresolved behavior `OPEN` in documentation and issues; do not silently
  choose between contradictory contracts.
- Preserve the service boundary: Auth owns identity and sessions, Backend owns
  recipe domain data, Frontend owns presentation/client state, and Index owns
  public static content.

## Frontend

- Use TypeScript for request and service-layer code and Vue single-file
  components for views.
- Keep HTTP calls behind the request modules and use the shared auth-header
  helper for protected Backend calls.
- Keep recipe and category transformations explicit at the boundary: the
  Backend uses comma-separated categories and a stringified recipe body, while
  the client uses arrays and parsed recipe data.
- Follow the existing ESLint baseline: Vue 3 essential rules, recommended
  JavaScript rules, and Vue TypeScript recommendations.
- Avoid adding a second global state mechanism without an explicit architecture
  decision; the current application uses global service instances and events.

## Backend

- Keep endpoint registration in `src/api/urls.py` and root mounting in
  `src/main/urls.py`.
- Validate request fields with DRF serializers before changing data.
- Apply the authenticated user filter to every protected domain read and write.
- Keep external AI and image calls bounded and observable when adding or
  changing integrations. Size, timeout, SSRF, and rate-limit rules are currently
  `OPEN` and should be resolved before expanding those integrations.
- Return a documented status and response shape. The existing API is not fully
  consistent, so a change should not copy an ambiguous neighboring endpoint.

## Auth

- Keep provider, email, cookie, trusted-origin, and JWT configuration in the
  Auth service rather than duplicating it in Frontend or Backend.
- Treat JWT verification as a security boundary. Explicitly define algorithm,
  issuer, audience, claims, lifetime, and revocation behavior before changing
  the validator.
- Keep credentials in deployment configuration and use the `.env.example` only
  as a variable-name reference.

## Tests and documentation

Document a new endpoint with authentication, request, response, status codes,
ownership, external calls, and data effects. Add or update tests when a contract
or security boundary changes. The currently visible test surface is recorded on
the [testing page](testing.md).

## Open standards

- `OPEN`: canonical formatter, Python lint configuration, commit convention,
  branch model, and review requirements.
- `OPEN`: the authoritative package manager and runtime versions across the
  repositories.
- `OPEN`: the expected contract-test strategy between Frontend and Backend.

## Further reading

- [Testing](testing.md)
- [API overview](../07-api/api-overview.md)
- [Repository structure](../01-overview/repository-structure.md)
- [Troubleshooting](../02-getting-started/troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English coding guidelines based on observed conventions |
