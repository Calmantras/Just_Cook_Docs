# Environment Variables

| Field | Value |
|---|---|
| Status | `CURRENT` for the variables visible in the reviewed examples; Backend variables are `OPEN` |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Frontend/.env.example`](../../../Just_Cook_Frontend/.env.example); [`Just_Cook_Auth/.env.example`](../../../Just_Cook_Auth/.env.example); [`Just_Cook_Auth/src/lib/auth.mjs`](../../../Just_Cook_Auth/src/lib/auth.mjs) |

## Purpose

This is a variable inventory, not a place to store values. Examples are
synthetic and must be replaced through a local or deployment secret mechanism.

## Frontend

| Variable | Purpose | Required status | Synthetic example |
|---|---|---|---|
| `VITE_BACKEND_URL` | Base URL used for Django API requests | Required by the example | `http://127.0.0.1:8000` |
| `VITE_AUTH_BACKEND_URL` | Base URL used by Better Auth client calls | Required by the example | `http://localhost:8005` |
| `VITE_S3_URL` | Prefix for stored image paths | Required by the example | `https://images.example.invalid` |
| `VITE_SHARE_URL` | Share-link base used by the frontend | `OPEN`; referenced by the client but absent from the examined example | `https://share.example.invalid` |

The client imports an `env` module with `backend`, `authBackend`, `s3`, and
`share` values. `src/env.ts` was not present in the analyzed tree, so the
mapping from Vite variables to that module must be resolved before relying on a
local build.

## Auth

| Variable | Purpose | Required status | Synthetic example |
|---|---|---|---|
| `BETTER_AUTH_SECRET` | Secret used by Better Auth for signing or encryption | Required | `<local-auth-secret>` |
| `BETTER_AUTH_URL` | Public/base URL of the Auth service | Required for non-default environments | `http://localhost:8005` |
| `DATABASE_URL` | PostgreSQL connection string | Required | `postgresql://app:<password>@localhost/just_cook_auth` |
| `TRUSTED_ORIGINS` | Comma-separated browser origins accepted by Auth and Express CORS | Required for cross-origin clients | `http://localhost:5173,http://localhost:8080` |
| `GOOGLE_CLIENT_ID` | Google OAuth client identifier | Required only if Google login is enabled | `<google-client-id>` |
| `GOOGLE_CLIENT_SECRET` | Google OAuth client secret | Required only if Google login is enabled | `<google-client-secret>` |
| `DISCORD_CLIENT_ID` | Discord OAuth client identifier | Required only if Discord login is enabled | `<discord-client-id>` |
| `DISCORD_CLIENT_SECRET` | Discord OAuth client secret | Required only if Discord login is enabled | `<discord-client-secret>` |
| `RESEND_API_KEY` | Resend API credential for verification and reset emails | Required for email delivery | `<resend-api-key>` |
| `EMAIL_FROM` | Sender identity for Auth emails | Required with Resend | `Just Cook <auth@example.invalid>` |

The Auth source applies defaults for `BETTER_AUTH_URL` and `TRUSTED_ORIGINS`, but
those defaults are not a substitute for an environment-specific decision.
Cookies are configured as `SameSite=None` and `Secure=true`, which requires a
browser context that supports secure cross-origin cookies.

## Backend and Index

`OPEN`: the accepted analysis does not provide a complete Backend environment
variable inventory or an Index variable contract. The Backend clearly requires
a database, Auth JWKS/base URL, image integration, Cerebras, and Groq
configuration, but the exact variable names and requiredness must be taken from
the current deployment configuration before being added here.

The Index has no application environment variables visible in the reviewed
Nginx, Docker, or Compose files.

## Handling rules

- Never commit `.env` files, access tokens, private keys, or real provider keys.
- Keep values out of Markdown examples, logs, screenshots, and issue reports.
- Use separate values and origins for local and production environments.
- Record a variable here when code starts depending on it, including whether it
  is required and which service consumes it.

## Further reading

- [Local development setup](local-development-setup.md)
- [Authentication overview](../03-auth/overview.md)
- [Deployment](../08-deployment/deployment.md)
- [Troubleshooting](troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial variable inventory with synthetic examples |
