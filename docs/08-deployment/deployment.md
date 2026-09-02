# Deployment

| Field | Value |
|---|---|
| Status | `CURRENT` container facts; complete release procedure is `OPEN` |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Index/Dockerfile`](../../../Just_Cook_Index/Dockerfile); [`Just_Cook_Index/compose.yml`](../../../Just_Cook_Index/compose.yml); [`Just_Cook_Frontend/Dockerfile`](../../../Just_Cook_Frontend/Dockerfile); [`Just_Cook_Frontend/compose.yml`](../../../Just_Cook_Frontend/compose.yml); [`Just_Cook_Backend/Dockerfile`](../../../Just_Cook_Backend/Dockerfile); [`Just_Cook_Backend/compose.yml`](../../../Just_Cook_Backend/compose.yml); [`Just_Cook_Auth/Dockerfile`](../../../Just_Cook_Auth/Dockerfile); [`Just_Cook_Auth/compose.yml`](../../../Just_Cook_Auth/compose.yml) |

## Purpose

This page records what the repository container definitions currently establish.
It is not yet a production runbook: release ownership, secret injection,
migrations, health checks, and rollback are not fully documented.

## Current container contracts

| Service | Image/build | Container port | Compose host mapping | Entrypoint |
|---|---|---:|---:|---|
| Index | `nginx:1.27-alpine` | 80 | `8080:80` | Nginx default command |
| Frontend | Bun builder, then `nginx:alpine` | 80 | `81:80` | Nginx default command |
| Backend | `python:3.13-slim` | 8000 | `8000:8000` | Gunicorn, 4 workers and 2 threads |
| Auth | `node:22-alpine` | 8005 | `8005:8005` | `node src/main.mjs` |

The recorded public domains are `just-cook.app`, `app.just-cook.net`,
`backend.just-cook.net`, and an environment-specific Auth domain. HTTPS and
the public reverse-proxy arrangement are not defined by these Compose files.

## Known build behavior

### Index

The image copies static HTML, the Nginx configuration, favicon files, and image
assets into the Nginx document root. Nginx serves static files and redirects
the two legal shortcuts. It does not proxy Auth or Backend traffic.

### Frontend

The multi-stage image installs dependencies with Bun, runs `bun run build`, and
copies `dist` into Nginx. SPA fallback routes are configured with
`try_files ... /index.html`.

`OPEN`: the Dockerfile uses `oven/bun:latest` and `--frozen-lockfile`, but no
visible `bun.lock` exists next to the checked-in `package-lock.json`. The
authoritative package manager and reproducible build inputs must be decided.

### Backend

The image installs `requirements.txt`, copies the repository, runs
`collectstatic` with a placeholder database URL during the image build, creates
an unprivileged `django` user, and starts Gunicorn on port `8000`.

The Dockerfile copies the complete build context. Do not place `.env` files or
other credentials in a build context until the ignore rules and secret-handling
process are verified.

### Auth

The image installs production npm dependencies from `package-lock.json`, copies
the `src` directory, runs as the `node` user, and starts Express on port `8005`.
The Compose file reads `.env`, sets `NODE_ENV=production`, and provides a host
gateway entry for database access.

## Deployment skeleton

The following sequence reflects dependency direction, not an executed or
approved release procedure:

1. Resolve environment domains, trusted origins, secret injection, and database
   ownership for the target environment.
2. Prepare the Auth PostgreSQL schema and verify the Better Auth migration
   process. This step is `OPEN` because no migration command is documented.
3. Build and deploy Auth; verify its database, email, OAuth, cookie, and JWKS
   behavior.
4. Configure and deploy the Backend; verify database access and Auth JWKS
   retrieval before accepting protected traffic.
5. Build and deploy the Frontend with URLs matching the deployed Auth, Backend,
   image, and share services.
6. Deploy the Index and verify static routes, legal content, and app links.
7. Perform smoke checks using synthetic/test accounts and data.

## Rollback and operations

`OPEN`: no approved rollback steps, migration rollback policy, backup/restore
procedure, health endpoint, monitoring, alerting, or release coordination
process is visible. Do not invent a rollback command for a schema or public
share change. Define whether database migrations are forward-only before the
first production deployment that changes data.

## Pre-release checklist

- No real credentials are present in source, build context, logs, or artifacts.
- Auth and Backend use the intended environment-specific URLs and origins.
- Better Auth schema state is known and backed up where required.
- JWT verification policy is safe and explicitly configured.
- Frontend runtime variables and the share URL are present.
- AI, image, email, and OAuth provider limits are configured.
- Public share lifetime and revocation behavior are understood.
- Static Index links and legal content are available without unintended third-
  party failure.

## Further reading

- [Local development setup](../02-getting-started/local-development-setup.md)
- [Environment variables](../02-getting-started/environment-variables.md)
- [System architecture](../01-overview/system-architecture.md)
- [Troubleshooting](../02-getting-started/troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial deployment skeleton and container inventory |
