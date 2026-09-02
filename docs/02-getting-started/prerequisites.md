# Prerequisites

| Field | Value |
|---|---|
| Status | `CURRENT` with `OPEN` version choices |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Frontend/package.json`](../../../Just_Cook_Frontend/package.json); [`Just_Cook_Frontend/Dockerfile`](../../../Just_Cook_Frontend/Dockerfile); [`Just_Cook_Backend/Dockerfile`](../../../Just_Cook_Backend/Dockerfile); [`Just_Cook_Auth/Dockerfile`](../../../Just_Cook_Auth/Dockerfile) |

## Purpose

This page lists what a developer needs before attempting the service-by-service
setup. It does not claim that the complete stack has a verified one-command
bootstrap.

## Required capabilities

- Git access to the four independent repositories.
- A recent Docker installation with Compose support for container-based work.
- Node.js and npm for the Auth service; the Auth image is based on Node 22.
- Python tooling compatible with the Backend image; the Dockerfile is based on
  Python 3.13.
- A frontend JavaScript toolchain capable of running the dependencies in
  `package.json`.
- Access to PostgreSQL for Auth and Backend development, unless an approved
  shared development database is used.
- Browser access to the local frontend and network access to configured Auth,
  API, image, email, OAuth, and AI services as applicable.

## Repository-specific prerequisites

| Repository | Observed dependency source or runtime | Local dependency status |
|---|---|---|
| Index | Nginx Alpine container | Static files only |
| Frontend | Node package manifest; Docker build uses `oven/bun:latest` | `OPEN`: package manager and lockfile authority |
| Backend | `python:3.13-slim`; requirements installed by pip | `OPEN`: exact local Python setup and binding dependency file |
| Auth | Node 22 Alpine; `package-lock.json` and npm | Database, mail, and OAuth configuration required |

## Before setup

1. Clone all four repositories next to this documentation repository.
2. Review [environment variables](environment-variables.md) without copying
   values from any existing `.env` file.
3. Decide which local URLs and browser origins will be used. This is currently
   `OPEN` and must be made consistent across Frontend and Auth.
4. Confirm that the required PostgreSQL databases and external providers are
   available or use approved local substitutes.

## Not established

- `OPEN`: supported operating systems and minimum versions.
- `OPEN`: authoritative Node, Bun, Python, and package-manager versions for
  non-container development.
- `OPEN`: whether local development requires running all four services or may
  use remote Auth, API, image, and AI dependencies.

## Further reading

- [Local development setup](local-development-setup.md)
- [Environment variables](environment-variables.md)
- [Repository structure](../01-overview/repository-structure.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English prerequisites page |
