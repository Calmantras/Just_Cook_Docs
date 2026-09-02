# Troubleshooting

| Field | Value |
|---|---|
| Status | `CURRENT` known-issue register |
| Last reviewed | 2026-09-02 |
| Sources | Linked source files below; static repository inventory reviewed on 2026-09-02 |

## Purpose

Use this page for issues that can block development, deployment, or safe use.
It centralizes known defects and contract mismatches instead of repeating them
throughout the service pages.

## Priority issues

| Priority | Symptom or risk | Current evidence | Action |
|---|---|---|---|
| P0 | JWT verification accepts the algorithm from the unchecked token header and does not validate issuer or audience | [`token_manager.py`](../../../Just_Cook_Backend/src/api/util/user/token_manager.py) | Fix the validation policy before treating the API as production-safe |
| P0 | Recipe cache key is not user-specific and is not cleared on logout | [`recipes.service.ts`](../../../Just_Cook_Frontend/src/public/ts/services/recipes.service.ts); settings route | Scope cache data to the account and clear it during sign-out |
| P0 | Real secrets or tokens were found in source, `.env`, worktrees, or history during the inventory | Inventory finding; values are intentionally not reproduced here | Rotate exposed credentials and remove them from source and history |
| P1 | Frontend imports missing `src/env.ts`; `VITE_SHARE_URL` is not in the examined example | [`auth-client.ts`](../../../Just_Cook_Frontend/src/lib/auth-client.ts); [`base.request.ts`](../../../Just_Cook_Frontend/src/public/ts/requests/base.request.ts) | Define and document the runtime configuration contract |
| P1 | Auth schema/migrations and startup order are not documented or automated | [`auth.mjs`](../../../Just_Cook_Auth/src/lib/auth.mjs); [`Dockerfile`](../../../Just_Cook_Auth/Dockerfile) | Define the schema lifecycle and deployment ordering |
| P1 | Frontend and Backend disagree on individual-recipe, category, and AI response field names | [`recipe.request.ts`](../../../Just_Cook_Frontend/src/public/ts/requests/recipe.request.ts); Backend recipe views | Agree on one response contract and add contract tests |
| P1 | Backend image build copies the complete context, including potentially sensitive files | [`Dockerfile`](../../../Just_Cook_Backend/Dockerfile) | Narrow the build context or strengthen `.dockerignore` and rotate any exposed values |
| P1 | Frontend Docker build uses Bun frozen-lockfile behavior without a visible Bun lockfile | [`Dockerfile`](../../../Just_Cook_Frontend/Dockerfile); [`package-lock.json`](../../../Just_Cook_Frontend/package-lock.json) | Decide the authoritative package manager and lockfile |
| P1 | Marketing pricing and credit claims are not technically enforced by the app | Index page and [`subscribe.vue`](../../../Just_Cook_Frontend/src/routes/subscribe.vue) | Mark product claims accurately until billing and entitlements exist |
| P2 | Public share snapshots have no visible expiry, revocation flag, relationship, or delete endpoint | [`view_recipe/models.py`](../../../Just_Cook_Backend/src/api/view_recipe/models.py); recipe views | Define share lifetime and revocation behavior |
| P2 | URL imports and AI/image payloads have no robust size, timeout, rate, or SSRF policy | Backend AI utilities | Add bounded inputs, timeouts, rate limits, and URL controls |
| P2 | Tests, CI, health checks, monitoring, backups, restore, and rollback are missing or incomplete | All four repositories | Define an operational baseline |

## Contract symptoms

### A recipe detail page is empty or malformed

The list request consumes `body` and `categories`. The frontend detail request
currently reads `recipemd` and `category`, which the reviewed Backend serializer
does not provide. Treat this as a contract mismatch, not as a data-repair task.

### An AI response cannot be parsed

OCR is wrapped in an `ocrtext` response field. The current image transformation
code and prompt do not consistently agree on JSON versus XML. Check the raw
response shape and the consuming frontend parser before changing either side.

### Authenticated requests work in one browser but not another

Check the Auth `TRUSTED_ORIGINS`, the frontend Auth URL, secure cross-origin
cookie behavior, and the locally stored `just-cook.jwt`. A stored JWT can keep
the PWA signed in even when the session cookie is unavailable, which can mask a
cookie or CORS problem.

### A share page does not load

The public data endpoint requires a `secret` query parameter. A missing secret
returns `400`; an unknown secret returns `404`. There is no documented TTL or
revocation route in the current API.

## OPEN decisions

- Which local, staging, and production URLs and origins are canonical?
- Which JWT claims and validation policy are binding?
- Is the recipe body JSON, XML, or another versioned contract?
- Are `body` and `categories` the stable Backend field names?
- What image dimensions, formats, and AI failure limits are allowed?
- What domain, lifetime, and deletion behavior apply to public shares?
- Are pricing, credits, billing, and native applications in scope?

## Further reading

- [API overview](../07-api/api-overview.md)
- [Authentication flow](../03-auth/authentication-flow.md)
- [Deployment](../08-deployment/deployment.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial centralized known-issue register |
