# Category Endpoints

| Field | Value |
|---|---|
| Status | `CURRENT` source-derived reference with `OPEN` value semantics |
| Last reviewed | 2026-09-02 |
| Authentication | Bearer JWT |
| Sources | [`Just_Cook_Backend/src/api/urls.py`](../../../../Just_Cook_Backend/src/api/urls.py); [`Just_Cook_Backend/src/api/view_category/views.py`](../../../../Just_Cook_Backend/src/api/view_category/views.py); [`Just_Cook_Backend/src/api/view_category/serializers.py`](../../../../Just_Cook_Backend/src/api/view_category/serializers.py); [`Just_Cook_Frontend/src/public/ts/requests/category.request.ts`](../../../../Just_Cook_Frontend/src/public/ts/requests/category.request.ts) |

## Purpose

These endpoints manage user-owned categories and the comma-separated category
value stored on recipes. All requests use:

```http
Content-Type: application/json; charset=UTF-8
Authorization: Bearer <synthetic-token>
```

Category ownership is checked with the JWT user ID in the Backend.

## `POST /api/addcategory/`

Creates a category for the current user.

```json
{
  "category": "Weeknight"
}
```

**Response (`200`):** the current view returns an empty object:

```json
{}
```

**Response (`400`):** serializer errors. **Data effects:** creates a `Category`
with `owner` set to the JWT user ID. The frontend's service currently expects
to push response data, but the visible Backend response is empty; verify this
contract before relying on the optimistic update.

## `GET /api/getcategories/`

Lists categories owned by the current user. No request body is required.

**Response (`200`):**

```json
[
  { "id": 1, "category": "Weeknight" },
  { "id": 2, "category": "Vegetarian" }
]
```

**Data effects:** reads only and filters by `owner`.

## `PUT /api/editcategory/`

Renames an owned category.

```json
{
  "id": 1,
  "category": "Quick meals"
}
```

**Response (`200`):** the updated category object. **Response (`404`):** the
category does not exist for the current user. **Response (`400`):** validation
error. **Data effects:** updates only the owned category.

## `DELETE /api/deletecategory/`

Deletes an owned category by body ID.

```json
{
  "id": 1
}
```

**Response (`200`):**

```json
{
  "success": true
}
```

**Response (`404`):** category not found for the current user. **Response
(`400`):** validation error. **Data effects:** deletes the category record. The
effect on recipes whose `categories` string still contains the ID is not
visible and is therefore `OPEN`.

## `PUT /api/update_recipe_category/`

Replaces the category string on an owned recipe.

```json
{
  "recipeid": 42,
  "categories": "1,4"
}
```

**Response (`200`):**

```json
{
  "success": true
}
```

**Response (`400`):** validation error. The view looks up the recipe by the
authenticated user and ID. If the lookup returns no recipe, the current code
does not visibly convert that case into a clean `404`; this behavior is `OPEN`.

## Representation rules

The Backend stores category IDs as text such as `"1,4,7,-1"`. The frontend
converts this to `[1, 4, 7, -1]` and joins it before updates. Current client
special values are:

| Value | Frontend meaning |
|---|---|
| `-1` | Favorites |
| `-2` | All recipes / technical no-category value during creation |
| `-3` | No selection |
| Positive integer | Backend category ID |

The negative values are not confirmed as a stable Backend API contract.

## Open questions

- Should category membership be a relational or typed representation?
- What should happen to recipe references when a category is deleted?
- Are the negative client values allowed in persisted recipe data?
- Should category creation return the created object instead of `{}`?

## Further reading

- [API overview](../api-overview.md)
- [Backend data model](../../04-backend/data-model.md)
- [Recipe endpoints](recipes.md)
- [Frontend routes and state](../../05-frontend/routes-and-state.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial grouped category endpoint reference |
