# Backend Overview

| Field | Value |
|---|---|
| Status | `CURRENT` with `OPEN` operational details |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Backend/src/main/urls.py`](../../../Just_Cook_Backend/src/main/urls.py); [`Just_Cook_Backend/src/api/urls.py`](../../../Just_Cook_Backend/src/api/urls.py); [`Just_Cook_Backend/src/api/view_recipe/views.py`](../../../Just_Cook_Backend/src/api/view_recipe/views.py); [`Just_Cook_Backend/src/api/view_category/views.py`](../../../Just_Cook_Backend/src/api/view_category/views.py) |

## Purpose

The Backend is the Django/DRF service that owns recipes, categories, public
share snapshots, and the server-side image and AI integrations.

## Current service boundary

The root URL configuration mounts the API at `/api/` and Django admin at
`/admin/`. Recipe and category views are function-based DRF endpoints protected
by the local `auth()` decorator, which obtains the user ID from a Better Auth
JWT. The ID is handled as a string.

The current endpoint groups are:

- recipe CRUD and listing;
- OCR text extraction and OCR-to-recipe transformation;
- website-to-recipe import;
- public share snapshot creation and retrieval;
- category CRUD and recipe-category updates.

## External integrations

| Integration | Current use |
|---|---|
| PostgreSQL | Django domain data |
| Better Auth JWKS | JWT verification and user identity |
| Thumbor / image storage | Image upload and stored image paths |
| Cerebras | OCR and text-to-recipe transformation |
| Groq with `visit_website` | Website recipe import |

The API does not visibly proxy authentication sessions. The frontend calls Auth
directly, then calls the Backend with a bearer JWT.

## Request and response conventions

Protected requests normally use:

```http
Content-Type: application/json; charset=UTF-8
Authorization: Bearer <synthetic-token>
```

The inner recipe body is a JSON document stored as a string in `Recipe.body`.
Categories are stored as a comma-separated string. The Backend serializers do
not fully validate the inner recipe JSON or convert categories into a native
list.

## Ownership

Recipe reads, writes, deletes, and shares filter by the authenticated user ID.
Category operations filter by category owner. Public share retrieval is
deliberately unauthenticated and uses a secret query parameter.

## Planned or open

- `OPEN`: complete Backend environment-variable and migration documentation.
- `OPEN`: stable recipe, category, OCR, and website-import response contracts.
- `OPEN`: input size, timeout, rate-limit, and SSRF controls for external calls.
- `OPEN`: health checks, monitoring, backup/restore, and operational rollback.

## Further reading

- [Data model](data-model.md)
- [API overview](../07-api/api-overview.md)
- [Deployment](../08-deployment/deployment.md)
- [Troubleshooting](../02-getting-started/troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial English Backend overview |
