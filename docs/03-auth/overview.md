# Authentication Overview

| Field | Value |
|---|---|
| Status | `CURRENT` with `OPEN` token-policy decisions |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Auth/src/lib/auth.mjs`](../../../Just_Cook_Auth/src/lib/auth.mjs); [`Just_Cook_Auth/src/main.mjs`](../../../Just_Cook_Auth/src/main.mjs); [`Just_Cook_Frontend/src/lib/auth-client.ts`](../../../Just_Cook_Frontend/src/lib/auth-client.ts); [`Just_Cook_Backend/src/api/util/user/token_manager.py`](../../../Just_Cook_Backend/src/api/util/user/token_manager.py) |

## Purpose

This page explains the boundary between Better Auth and the recipe API. It is
not a replacement for the Better Auth version-specific route documentation.

## Current service

`Just_Cook_Auth` is an Express adapter around Better Auth. It listens on port
`8005` and forwards `/api/auth/{*any}` to Better Auth. The service uses a
PostgreSQL connection pool and configures the following capabilities:

- email/password sign-up and sign-in;
- mandatory email verification for email/password registration;
- password reset emails;
- automatic sign-in after email verification;
- Google and Discord social providers with implicit sign-up disabled;
- session operations and the JWT/JWKS plugin;
- CORS and Better Auth trusted origins from `TRUSTED_ORIGINS`;
- Resend for verification and password-reset emails.

The frontend creates a Better Auth Vue client using the configured Auth base
URL. It requests a JWT through the client token operation and stores the latest
token in browser `localStorage` under `just-cook.jwt` for PWA continuity.

## Cookie and origin behavior

Auth configures default cookies with `sameSite: "none"` and `secure: true` for
cross-domain use. Express CORS enables credentials and uses the configured
trusted origins. Local development must therefore use an origin matrix that
matches the browser and the Auth configuration.

## Security boundary

The Backend does not use a relational foreign key to Better Auth users. It
extracts a user ID from the JWT (`id` or `sub`) and uses that string to scope
recipes, categories, and shares. The current token-validation implementation
does not visibly pin the JWT algorithm or validate issuer and audience. This is
a production security issue and is centralized in
[troubleshooting](../02-getting-started/troubleshooting.md).

## Planned or open

- `OPEN`: the required JWT claims, algorithm, issuer, audience, lifetime, and
  revocation policy.
- `OPEN`: how Better Auth schema creation and migrations are generated, applied,
  and ordered relative to service startup.
- `OPEN`: whether email-verification and social-login consent behavior satisfies
  the final product and legal requirements.
- `OPEN`: the canonical Auth hostname for each environment.

## Further reading

- [Authentication flow](authentication-flow.md)
- [Auth endpoints](../07-api/endpoints/auth.md)
- [Environment variables](../02-getting-started/environment-variables.md)
- [Backend data model](../04-backend/data-model.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English Auth service overview |
