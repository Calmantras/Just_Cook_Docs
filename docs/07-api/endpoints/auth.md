# Authentication Endpoints

| Field | Value |
|---|---|
| Status | `CURRENT` route inventory; exact Better Auth contracts are `OPEN` |
| Last reviewed | 2026-09-02 |
| Authentication | Session cookie for session operations; provider-specific for sign-in; JWT returned by token operation |
| Sources | [`Just_Cook_Auth/src/main.mjs`](../../../../Just_Cook_Auth/src/main.mjs); [`Just_Cook_Auth/src/lib/auth.mjs`](../../../../Just_Cook_Auth/src/lib/auth.mjs); [`Just_Cook_Frontend/src/lib/auth-client.ts`](../../../../Just_Cook_Frontend/src/lib/auth-client.ts) |

## Purpose

`Just_Cook_Auth` forwards all requests below `/api/auth/` to Better Auth. The
exact route set and response schemas depend on the installed Better Auth
version; the following are the important routes used or configured by the
reviewed application.

## Base path and cross-origin request

The service listens on port `8005` and mounts the Better Auth handler at
`/api/auth/{*any}`. Browser requests use credentials because Auth configures
cross-origin cookies.

```http
Content-Type: application/json
Origin: http://localhost:5173
```

`Origin` must be present in `TRUSTED_ORIGINS` for a cross-origin browser client.
The actual local origin is `OPEN`.

## Email registration

### `POST /api/auth/sign-up/email`

Creates an email/password account through Better Auth.

```json
{
  "name": "Example User",
  "email": "user@example.invalid",
  "password": "<synthetic-password>"
}
```

The Auth configuration requires email verification and sends a link through
Resend. The precise success body, password policy, and duplicate-account error
schema are Better Auth version-specific.

**Data effects:** creates Better Auth user/account records and sends a
verification email. No Django recipe record is created.

**Important statuses:** `2xx` for an accepted registration; `4xx` for invalid
input, an existing account, or a disallowed origin; provider and mail failures
may surface as `5xx`. Exact status mapping is `OPEN`.

## Email sign-in

### `POST /api/auth/sign-in/email`

```json
{
  "email": "user@example.invalid",
  "password": "<synthetic-password>"
}
```

On success Better Auth establishes a session cookie. Email/password accounts
must satisfy the configured verification requirement. The frontend later calls
the token route to obtain a bearer JWT for the Django API.

**Data effects:** reads Auth records and may create or update a session. It does
not change recipe data.

**Important statuses:** `2xx` on success; `4xx` for invalid credentials,
unverified accounts, malformed input, or origin failures. Exact status and body
are `OPEN`.

## Social sign-in

### `POST /api/auth/sign-in/social`

Google and Discord are configured as social providers. The frontend delegates
the provider-specific request and callback behavior to Better Auth.

**Data effects:** may create or update a provider account and Auth session.

**Open details:** provider callback URLs, redirect parameters, consent, and the
behavior when implicit sign-up is disabled must be verified against the installed
Better Auth version and provider configuration.

## Session and token operations

### `GET /api/auth/get-session`

Returns the current Better Auth session when the session cookie is valid. The
frontend uses it to guard protected routes.

### `POST /api/auth/token`

The frontend calls the Better Auth client token operation and stores the
returned JWT as `just-cook.jwt`. The exact HTTP method and response envelope are
owned by the Better Auth version; the observed client contract is conceptually:

```json
{
  "token": "<synthetic-jwt>"
}
```

The token is then sent to the Backend as:

```http
Authorization: Bearer <synthetic-jwt>
```

### `POST /api/auth/sign-out`

Ends the Better Auth session. The frontend also removes its local
`just-cook.jwt` value through its client helper. Already-issued stateless JWTs
are not visibly revoked immediately.

## Verification, reset, and JWKS

Better Auth also exposes version-specific routes for email verification and
password reset. The Auth configuration sends both emails through Resend and
enables automatic sign-in after verification.

### `GET /api/auth/jwks`

Publishes the JSON Web Key Set used by the Django Backend to validate Auth JWTs.
This is a service-to-service read and does not represent a user data mutation.

## Open questions

- Which exact Better Auth route methods, request bodies, and response envelopes
  are binding for the installed version?
- Which JWT algorithm, issuer, audience, claims, lifetime, and revocation model
  must the Backend enforce?
- Which provider callback URLs and local/production origins are configured?
- How are Auth schema migrations created and applied?

## Further reading

- [Authentication overview](../../03-auth/overview.md)
- [Authentication flow](../../03-auth/authentication-flow.md)
- [API overview](../api-overview.md)
- [Environment variables](../../02-getting-started/environment-variables.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial grouped Auth endpoint reference |
