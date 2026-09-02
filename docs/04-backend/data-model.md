# Backend Data Model

| Field | Value |
|---|---|
| Status | `CURRENT` with `OPEN` contract decisions |
| Last reviewed | 2026-09-02 |
| Sources | [`Just_Cook_Backend/src/api/view_recipe/models.py`](../../../Just_Cook_Backend/src/api/view_recipe/models.py); [`Just_Cook_Backend/src/api/view_category/models.py`](../../../Just_Cook_Backend/src/api/view_category/models.py); [`Just_Cook_Backend/src/api/view_recipe/serializers.py`](../../../Just_Cook_Backend/src/api/view_recipe/serializers.py) |

## Purpose

This page documents the domain records used by the Django API and the wire
representations visible in the current source snapshot.

## Current model shape

```mermaid
erDiagram
    RECIPE {
        bigint id PK
        string user
        text title
        text body
        text image
        text categories
    }
    CATEGORY {
        bigint id PK
        string owner
        text category
    }
    RECIPE_SHARE {
        bigint id PK
        text share_secret
        string user
        text title
        text body
        text image
    }
    RECIPE }o..o CATEGORY : "IDs encoded in categories"
    RECIPE ||..o{ RECIPE_SHARE : "snapshot copied, not FK"
```

The diagram shows application meaning, not Django foreign keys. The reviewed
models contain no relational link between recipes and categories, no foreign
key to a Better Auth user, and no relational link from `RecipeShare` back to a
recipe.

## Recipe

| Field | Type in model | Meaning |
|---|---|---|
| `id` | `BigAutoField` | Primary key |
| `user` | `CharField(100)` | Better Auth user ID as a string |
| `title` | `TextField` | Recipe title |
| `body` | `TextField` | String containing the recipe JSON |
| `image` | `TextField` | `none`, an image path, or input image data during creation |
| `categories` | Nullable `TextField` | Comma-separated category IDs |

The frontend expects the `body` string to parse into:

```json
{
  "recipe_title": "Pasta",
  "ingredients": ["200 g pasta"],
  "instructions": ["Boil the pasta"]
}
```

The Backend serializer exposes model fields and does not validate this inner
JSON structure. The frontend uses `-1` for favorites, `-2` for all recipes, and
`-3` for no selection. These special values are a client convention, not a
confirmed domain contract.

## Category

`Category` has an auto-incrementing `id`, an `owner` string containing the Auth
user ID, and the category name in `category`. The API returns category objects
with `id` and `category` for list and edit operations.

## RecipeShare

Creating a share copies the current user, title, body, and image into a new
snapshot with a random URL-safe `share_secret`. Public retrieval returns title,
body, and image. The current model has no visible expiration, revocation flag,
source recipe relationship, or delete endpoint.

## Open contract questions

- Is the JSON recipe body the stable format, or is XML still intended by the AI
  transformation prompt?
- Are `body` and `categories` the only stable API names? The frontend detail
  request currently reads `recipemd` and `category`.
- Should categories become a relational or typed representation?
- What retention and revocation policy applies to share snapshots?

## Further reading

- [Backend overview](overview.md)
- [Recipe endpoints](../07-api/endpoints/recipes.md)
- [Category endpoints](../07-api/endpoints/categories.md)
- [Troubleshooting](../02-getting-started/troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial model and wire-format description |
