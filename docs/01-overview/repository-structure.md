# Repository Structure

| Field | Value |
|---|---|
| Status | `CURRENT` |
| Last reviewed | 2026-09-02 |
| Sources | Repository Dockerfiles, Compose files, and source trees linked below; static inventory reviewed on 2026-09-02 |

## Purpose

Use this page to select the repository for a change and to understand the
runtime boundary around it.

## Repository map

| Repository | Change here when you need to... | Key files |
|---|---|---|
| `Just_Cook_Index` | Change marketing, legal loading, static assets, or static routing | [`index.html`](../../../Just_Cook_Index/index.html), [`legal.html`](../../../Just_Cook_Index/legal.html), [`nginx.conf`](../../../Just_Cook_Index/nginx.conf) |
| `Just_Cook_Frontend` | Change views, routes, API requests, client state, PWA behavior, or Capacitor integration | [`src/router.ts`](../../../Just_Cook_Frontend/src/router.ts), [`src/public/ts/requests/`](../../../Just_Cook_Frontend/src/public/ts/requests), [`src/public/ts/services/`](../../../Just_Cook_Frontend/src/public/ts/services) |
| `Just_Cook_Backend` | Change REST endpoints, models, ownership checks, AI calls, image handling, or Django admin | [`src/main/urls.py`](../../../Just_Cook_Backend/src/main/urls.py), [`src/api/urls.py`](../../../Just_Cook_Backend/src/api/urls.py), [`src/api/view_recipe/`](../../../Just_Cook_Backend/src/api/view_recipe) |
| `Just_Cook_Auth` | Change Better Auth configuration, email/OAuth providers, cookies, JWT, or CORS | [`src/lib/auth.mjs`](../../../Just_Cook_Auth/src/lib/auth.mjs), [`src/main.mjs`](../../../Just_Cook_Auth/src/main.mjs) |

## Runtime relationship

The index is a static Nginx site and does not proxy application requests. The
frontend is a separately served SPA. The frontend talks directly to the Auth
and Backend public URLs, while images are resolved through the configured image
host. The Auth service exposes Better Auth below `/api/auth/`; the Backend
exposes application endpoints below `/api/`.

## Source ownership rules

- Keep cross-service contracts synchronized when changing a request, response,
  token claim, or environment variable.
- Keep secrets in deployment configuration, not in source or documentation.
- Treat the source repository READMEs and old documentation as historical where
  they conflict with the reviewed implementation. The current API pages link
  to source files instead.
- There is no documented shared release pipeline across the four repositories.
  Coordinate release order manually until that process is defined.

## Open questions

`OPEN`: the binding runtime versions and package-manager choices are not fully
consistent across the repositories. The frontend Dockerfile uses Bun and
`--frozen-lockfile`, but the repository contains `package-lock.json` and no
visible `bun.lock`. Do not silently select one as the project-wide authority.

## Further reading

- [Project overview](project-overview.md)
- [Environment variables](../02-getting-started/environment-variables.md)
- [Deployment](../08-deployment/deployment.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English repository map |
