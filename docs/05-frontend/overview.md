# Frontend Overview

| Field | Value |
|---|---|
| Status | `CURRENT` with `OPEN` build and runtime configuration |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Frontend/src/index.vue`](../../../Just_Cook_Frontend/src/index.vue); [`Just_Cook_Frontend/src/lib/auth-client.ts`](../../../Just_Cook_Frontend/src/lib/auth-client.ts); [`Just_Cook_Frontend/src/public/ts/requests/base.request.ts`](../../../Just_Cook_Frontend/src/public/ts/requests/base.request.ts); [`Just_Cook_Frontend/package.json`](../../../Just_Cook_Frontend/package.json) |

## Purpose

The Frontend is the Vue 3 and Ionic Vue client. It runs as a browser SPA and
has Capacitor and PWA support in the repository. It owns presentation, routing,
client-side recipe/category state, request wrappers, and browser persistence.

## Current capabilities

- email/password, Google, and Discord authentication screens;
- registration, verification information, and password reset screens;
- recipe list, fuzzy search, category filtering, and favorites;
- recipe create, edit, delete, and public-share actions;
- image capture, OCR, and website import entry points;
- timers for time specifications in preparation instructions;
- PWA installation and cache reset behavior;
- a static subscription/pricing preview without visible billing enforcement.

## Client integration

The request layer wraps `CapacitorHttp`, adds JSON content type and bearer
authorization headers, and calls the configured Backend base URL. The Auth
client is created with the configured Auth base URL and the Better Auth JWT
plugin. Image paths are resolved by prefixing the configured S3/image URL.

There is no formal Pinia or Vuex store. Global class instances named
`recipes_service` and `categories_service` hold state and notify components with
`CustomEvent` events.

## Build and serving boundary

Vite is the development/build tool named in `package.json`. The production
Dockerfile uses a Bun builder and serves the generated files from Nginx on port
80; its Compose file maps host port `81` to that port.

`OPEN`: `src/env.ts` is imported by the examined source but is not present in the
tree, and the Dockerfile expects frozen-lockfile behavior without a visible Bun
lockfile. Resolve both before treating the production image as reproducible.

## Planned or open

- `OPEN`: exact environment-module implementation and the share URL variable.
- `OPEN`: authoritative package manager, lockfile, and runtime versions.
- `OPEN`: whether Capacitor targets are supported native applications or only
  repository scaffolding.
- `OPEN`: the implementation status of billing, credits, plans, and entitlements.

## Further reading

- [Routes and state](routes-and-state.md)
- [Authentication flow](../03-auth/authentication-flow.md)
- [API overview](../07-api/api-overview.md)
- [Troubleshooting](../02-getting-started/troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English Frontend overview |
