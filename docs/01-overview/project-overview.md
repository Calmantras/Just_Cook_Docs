# Project Overview

| Field | Value |
|---|---|
| Status | `CURRENT` with `OPEN` product decisions |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Index/index.html`](../../../Just_Cook_Index/index.html); [`Just_Cook_Frontend/src/router.ts`](../../../Just_Cook_Frontend/src/router.ts); [`Just_Cook_Backend/src/api/urls.py`](../../../Just_Cook_Backend/src/api/urls.py); [`Just_Cook_Auth/src/main.mjs`](../../../Just_Cook_Auth/src/main.mjs) |

## Purpose

This page gives a short answer to what Just Cook is and where each technical
responsibility lives. It is the entry point for developers who do not yet know
which repository to change.

## Current

Just Cook is a recipe application with these user-facing capabilities in the
reviewed state:

- account registration and email/password or social sign-in;
- recipe listing, fuzzy search, category filtering, favorites, and CRUD;
- image capture, OCR, OCR-to-recipe transformation, and website import;
- public recipe sharing;
- timers derived from preparation-step time specifications;
- PWA installation and cache reset;
- a public landing page and legal content.

The system is split into four repositories rather than one deployable unit:

| Part | Responsibility | Observed runtime |
|---|---|---|
| `Just_Cook_Index` | Static marketing and legal pages | Nginx on HTTP 80; Compose maps host 8080 |
| `Just_Cook_Frontend` | Vue 3 and Ionic SPA, routing, browser/PWA behavior, API client | Vite during development; Nginx serves the container on HTTP 80 |
| `Just_Cook_Backend` | Django/DRF recipe, category, sharing, image, and AI API | Gunicorn on HTTP 8000 |
| `Just_Cook_Auth` | Better Auth accounts, sessions, email verification, OAuth, JWT, and JWKS | Express on HTTP 8005 |

The recorded public production domains are `just-cook.app` for the index,
`app.just-cook.net` for the frontend, `backend.just-cook.net` for the API, and
an environment-specific auth domain. Image paths use `images.just-cook.net`.

## Boundaries

The index has no recipe database or authentication. The frontend calls the Auth
service for account operations and the Django API for domain data. The backend
uses the user ID from a Better Auth JWT as a string; it does not show a
relational user model shared with Better Auth.

## Planned or not established

- `PLANNED`: a runnable, verified local setup guide is a later documentation
  step; this first version intentionally provides a safe skeleton.
- `OPEN`: whether pricing, credits, billing, and entitlements are implemented
  or only marketing content.
- `OPEN`: whether the supported product is a PWA, a native Android app, a native
  iOS app, or all of them.
- `OPEN`: the canonical domain matrix for local, staging, and production.

## Further reading

- [System architecture](system-architecture.md)
- [Repository structure](repository-structure.md)
- [Local development setup](../02-getting-started/local-development-setup.md)
- [API overview](../07-api/api-overview.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English documentation page |
