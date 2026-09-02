# Local Development Setup

| Field | Value |
|---|---|
| Status | `CURRENT` skeleton; runnable procedure is `OPEN` |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Index/compose.yml`](../../../Just_Cook_Index/compose.yml); [`Just_Cook_Frontend/compose.yml`](../../../Just_Cook_Frontend/compose.yml); [`Just_Cook_Backend/compose.yml`](../../../Just_Cook_Backend/compose.yml); [`Just_Cook_Auth/compose.yml`](../../../Just_Cook_Auth/compose.yml) |

## Purpose

This page gives a safe order for bringing the four repositories into a local
workspace. It is a setup skeleton, not a tested runbook. Commands that depend
on unresolved runtime or environment decisions are intentionally marked `OPEN`.

## Workspace layout

Place the repositories as siblings:

```text
workspace/
├── Just_Cook_Docs/
├── Just_Cook_Index/
├── Just_Cook_Frontend/
├── Just_Cook_Backend/
└── Just_Cook_Auth/
```

## Service order

### 1. Auth service

The Auth service listens on port `8005` and mounts Better Auth at
`/api/auth/{*any}`. Prepare a local `.env` from the repository's example,
configure a PostgreSQL connection, and provide the required secret and provider
values. The Compose file maps `8005:8005` and reads `.env`.

`OPEN`: the database migration or schema initialization command and the correct
startup order are not documented in the source snapshot. Do not infer a
migration workflow from the presence of PostgreSQL alone.

### 2. Backend service

The Django service listens on `8000`, mounts its application API at `/api/`, and
uses `8000:8000` in Compose. It must be able to validate tokens through the
Better Auth JWKS URL and connect to its configured database and external image
and AI providers.

`OPEN`: the complete Backend environment-variable list, local database
initialization, migration command, and trusted-origin configuration are not
available in this analysis snapshot.

### 3. Frontend service

The frontend uses Vite for development and serves a production build through
Nginx. Its Compose file maps host port `81` to container port `80`. The
frontend needs the Backend URL, Auth URL, and image URL before it can make
useful authenticated requests.

`OPEN`: `src/env.ts` is imported by the client but is not present in the
examined tree. The source of the `share` URL and the exact development command
for the current checkout must be resolved before this becomes a runnable guide.

### 4. Index service

The index is independent of the application stack. Its Compose file builds the
Nginx image and maps host port `8080` to port `80`. It can be served separately
once the static files are available.

## Known container commands

These commands are recorded from the Compose files, but were not executed as
part of the documentation pass:

```sh
# Run from the corresponding repository directory.
docker compose up --build
```

The command is structurally applicable to each repository with a Compose file.
`OPEN`: whether the containers are expected to share a network, external
database, reverse proxy, or startup dependency in a real local environment.

## Verification checklist

After the environment decisions are resolved, verify in this order:

1. Auth responds on port `8005` and can access its database and mail provider.
2. Backend responds on port `8000` and can retrieve the Auth JWKS document.
3. Frontend loads and its configured URLs point to the intended services.
4. A test account can authenticate without exposing a real credential.
5. A protected API request reaches the Backend with a bearer JWT.
6. Index links resolve to the intended frontend and legal pages.

## Further reading

- [Prerequisites](prerequisites.md)
- [Environment variables](environment-variables.md)
- [Deployment](../08-deployment/deployment.md)
- [Troubleshooting](troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial service-by-service setup skeleton |
