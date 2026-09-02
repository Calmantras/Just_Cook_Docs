# API Overview

| Field | Value |
|---|---|
| Status | `CURRENT` register with `OPEN` contract details |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Backend/src/main/urls.py`](../../../Just_Cook_Backend/src/main/urls.py); [`Just_Cook_Backend/src/api/urls.py`](../../../Just_Cook_Backend/src/api/urls.py); [`Just_Cook_Frontend/src/public/ts/requests/`](../../../Just_Cook_Frontend/src/public/ts/requests) |

## Purpose

The Django API is mounted at `/api/` on the Backend host. This page is a
navigation-level register; grouped pages document the request and response
shapes without claiming a finalized public contract.

## Common request rules

Protected recipe and category endpoints currently use:

```http
Content-Type: application/json; charset=UTF-8
Authorization: Bearer <synthetic-token>
```

The Backend reads the user ID from the Better Auth JWT (`id` or `sub`) and
filters domain records by that string. There is no visible role-based API
authorization. Some endpoints use `POST` or `DELETE` bodies for IDs rather than
path parameters.

## Endpoint groups

### Auth

Better Auth is exposed separately by `Just_Cook_Auth` under `/api/auth/`.
See [authentication endpoints](endpoints/auth.md).

### Recipes and AI

| Method | Path | Auth | Purpose |
|---|---|---|---|
| `GET` | `/api/getrecipes/` | Bearer JWT | List the current user's recipes |
| `POST` | `/api/getrecipe/` | Bearer JWT | Load one recipe by body `id` |
| `POST` | `/api/postrecipe/` | Bearer JWT | Create a recipe |
| `PUT` | `/api/editrecipe/` | Bearer JWT | Update a recipe |
| `DELETE` | `/api/deleterecipe/` | Bearer JWT | Delete a recipe by body `id` |
| `POST` | `/api/getocrtext/` | Bearer JWT | OCR a Base64 image |
| `POST` | `/api/ocrtorecipe/` | Bearer JWT | Transform OCR text into a recipe |
| `POST` | `/api/webtorecipe/` | Bearer JWT | Import a recipe from a website |
| `POST` | `/api/sharerecipe/` | Bearer JWT | Create a public snapshot secret |
| `GET` | `/api/getsharerecipe/` | Public | Render the shared recipe HTML |
| `GET` | `/api/getsharerecipe/data/` | Public | Return data for a share secret |

See [recipe endpoints](endpoints/recipes.md).

### Categories

| Method | Path | Auth | Purpose |
|---|---|---|---|
| `POST` | `/api/addcategory/` | Bearer JWT | Create a category |
| `GET` | `/api/getcategories/` | Bearer JWT | List the user's categories |
| `PUT` | `/api/editcategory/` | Bearer JWT | Rename a category |
| `DELETE` | `/api/deletecategory/` | Bearer JWT | Delete a category |
| `PUT` | `/api/update_recipe_category/` | Bearer JWT | Replace a recipe's category string |

See [category endpoints](endpoints/categories.md).

## Not currently represented as resources

The reviewed API has no separate user, ingredient, plan, billing, entitlement,
or role-management resource. Pricing, credits, and billing scope are `OPEN`.

## Cross-cutting contract issues

- Recipe body is a JSON document stored as a string, without full Backend
  validation.
- Categories are a comma-separated string; negative values are not a confirmed
  Backend domain contract.
- Recipe list and detail consumers disagree on response field names.
- OCR and website-import response contents are wrapped under `ocrtext`, but the
  inner format is not consistent across producers and consumers.
- Public share links have no documented expiry or revocation route.

## Further reading

- [Backend overview](../04-backend/overview.md)
- [Backend data model](../04-backend/data-model.md)
- [Troubleshooting](../02-getting-started/troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial grouped API register |
