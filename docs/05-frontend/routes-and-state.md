# Frontend Routes and State

| Field | Value |
|---|---|
| Status | `CURRENT` with `OPEN` contract decisions |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Frontend/src/router.ts`](../../../Just_Cook_Frontend/src/router.ts); [`Just_Cook_Frontend/src/public/ts/services/recipes.service.ts`](../../../Just_Cook_Frontend/src/public/ts/services/recipes.service.ts); [`Just_Cook_Frontend/src/public/ts/services/categories.service.ts`](../../../Just_Cook_Frontend/src/public/ts/services/categories.service.ts) |

## Purpose

This page explains how the SPA protects routes and where recipe/category state
is held. It is useful when changing navigation or a page that consumes shared
state.

## Routes

| Route | Visibility | Component or role |
|---|---|---|
| `/` | Redirect | Redirects to `/recipes` |
| `/login` | Public | Email/password sign-in |
| `/register` | Public | Registration and social sign-in entry point |
| `/legal` | Public | Application legal page |
| `/reset` | Public | Start password reset |
| `/reset-password` | Public | Set a new password |
| `/verify-email` | Public | Email verification information |
| `/recipes` | Protected by default | Recipe list |
| `/recipe/:id` | Protected by default | Recipe detail |
| `/new_recipe` | Protected by default | Create recipe |
| `/settings` | Protected by default | Account and application settings |
| `/add_category` | Protected by default | Category management page |
| `/subscribe` | Protected by default | Static subscription preview |
| `/:pathMatch(.*)*` | Public | Not-found page |

Routes are protected unless they explicitly set `meta.requiresAuth: false`.
Before entering a protected route, the router checks the Better Auth session or
falls back to the stored JWT. If neither is available, it redirects to
`/login`.

## Recipe state

`recipes_service` maintains three arrays:

- `recipes`: all locally loaded recipes for the current user;
- `show`: recipes after the selected category filter;
- `search`: the current fuzzy-search result.

On refresh it may render `localStorage` cache key `just-cook.recipes` first,
then loads recipes and categories from the API. A successful recipe response
replaces the cache. The service parses the JSON string in `body` and expects
`recipe_title`, `ingredients`, and `instructions`.

The service dispatches `recipe_change` after filtering. Category state dispatches
`category_change`; category IDs `-1`, `-2`, and `-3` currently represent
favorites, all recipes, and no selection respectively.

## Category state

`categories_service` holds the category list and the selected category. It
converts the Backend comma-separated category string to `number[]` for the
client and joins the list again for updates. Positive values represent stored
category IDs; the negative values are client-side special selections.

## Persistence and logout

The JWT is stored under `just-cook.jwt`. Recipe data is stored under a single
global cache key. The reviewed code does not scope that cache key to a user or
clear it on logout, creating a cross-account data exposure risk on shared
browsers. Treat this as a priority issue; see
[troubleshooting](../02-getting-started/troubleshooting.md).

## Open contract issues

The list request reads Backend fields `body` and `categories`, while the detail
request reads `recipemd` and `category`. The current Backend serializer exposes
the former names. Align the contract before changing detail-page parsing.

## Further reading

- [Frontend overview](overview.md)
- [Authentication flow](../03-auth/authentication-flow.md)
- [Recipe endpoints](../07-api/endpoints/recipes.md)
- [Backend data model](../04-backend/data-model.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial route and client-state description |
