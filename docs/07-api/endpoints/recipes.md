# Recipe Endpoints

| Field | Value |
|---|---|
| Status | `CURRENT` source-derived reference with `OPEN` response mismatches |
| Last reviewed | 2026-09-02 |
| Authentication | Bearer JWT for all endpoints except public share retrieval |
| Sources | [`Just_Cook_Backend/src/api/urls.py`](../../../../Just_Cook_Backend/src/api/urls.py); [`Just_Cook_Backend/src/api/view_recipe/views.py`](../../../../Just_Cook_Backend/src/api/view_recipe/views.py); [`Just_Cook_Backend/src/api/view_recipe/serializers.py`](../../../../Just_Cook_Backend/src/api/view_recipe/serializers.py); [`Just_Cook_Frontend/src/public/ts/requests/recipe.request.ts`](../../../../Just_Cook_Frontend/src/public/ts/requests/recipe.request.ts) |

## Purpose

These endpoints cover recipe CRUD, AI-assisted ingestion, and public sharing.
Unless stated otherwise, protected requests use:

```http
Content-Type: application/json; charset=UTF-8
Authorization: Bearer <synthetic-token>
```

The Backend scopes protected recipe operations to the user ID from the JWT.
The API uses body fields for recipe IDs instead of path parameters.

## Recipe representation

The current model-level response includes:

```json
{
  "id": 42,
  "user": "user-123",
  "title": "Pasta",
  "body": "{\"recipe_title\":\"Pasta\",\"ingredients\":[\"200 g pasta\"],\"instructions\":[\"Boil the pasta\"]}",
  "image": "recipes/pasta.jpg",
  "categories": "1,4"
}
```

The value of `body` is a JSON string, not a nested JSON object. The Backend does
not fully validate its inner structure. `image` is commonly `none` or a stored
image path. `categories` is a comma-separated string.

## `GET /api/getrecipes/`

Returns all recipes whose `user` equals the authenticated user ID.

**Request:** no body.

**Response (`200`):** an array of recipe model objects, for example:

```json
[
  {
    "id": 42,
    "user": "user-123",
    "title": "Pasta",
    "body": "{\"recipe_title\":\"Pasta\",\"ingredients\":[],\"instructions\":[]}",
    "image": "none",
    "categories": "1,4"
  }
]
```

**Data effects:** reads only; ownership is applied by the Backend query.

## `POST /api/getrecipe/`

Loads one recipe by body ID for the authenticated user.

```json
{
  "id": 42
}
```

**Response (`200`):** one serialized recipe model object. **Response (`404`):**
no recipe for that ID and user.

**Data effects:** reads only and checks ownership through the user filter.

`OPEN`: the frontend detail request currently reads `recipemd` and `category`,
while the Backend serializer exposes `body` and `categories`. This mismatch can
make a valid response appear empty or malformed in the detail page.

## `POST /api/postrecipe/`

Creates a recipe for the authenticated user.

```json
{
  "title": "Pasta",
  "body": "{\"recipe_title\":\"Pasta\",\"ingredients\":[\"200 g pasta\"],\"instructions\":[\"Boil the pasta\"]}",
  "image": "none",
  "categories": "1,4"
}
```

The frontend sends categories by joining a number array with commas. New images
may be Base64 strings; the Backend sends non-`none` images to its image helper.

**Response (`201`):** the serialized data with the stored image path and newly
assigned ID. **Response (`400`):** serializer errors.

**Data effects:** creates a `Recipe` owned by the JWT user and may upload an
image through Thumbor/image storage.

## `PUT /api/editrecipe/`

Updates a recipe by body ID.

```json
{
  "id": 42,
  "title": "Updated pasta",
  "body": "{\"recipe_title\":\"Updated pasta\",\"ingredients\":[],\"instructions\":[]}",
  "image": "recipes/pasta.jpg",
  "categories": "1,7"
}
```

**Response (`200`):** serializer data after validation. **Response (`404`):**
the current view returns this when input validation fails; a missing matching
recipe is not clearly distinguished. **Data effects:** updates the owned
recipe and may upload a replacement image.

## `DELETE /api/deleterecipe/`

Deletes a recipe by body ID.

```json
{
  "id": 42
}
```

**Response (`200`):**

```json
{
  "success": true
}
```

**Response (`400`):** invalid ID. **Data effects:** deletes only a recipe
filtered by the authenticated user; no visible image cleanup is documented.

## `POST /api/getocrtext/`

Sends a Base64 image for OCR.

```json
{
  "image": "<base64-image-without-real-data>"
}
```

**Response (`200`):**

```json
{
  "ocrtext": "<synthetic-ocr-result>"
}
```

**Response (`400`):** input validation error. **Data effects:** the Backend
passes image data to the image helper, which uploads/saves the image and calls
Cerebras for OCR. Input size, format, timeout, and rate limits are `OPEN`.

## `POST /api/ocrtorecipe/`

Transforms OCR text into a recipe-shaped result.

```json
{
  "ocr": "<synthetic-ocr-text>"
}
```

**Response (`201`):** the transformation result directly. **Response (`400`):**
input validation error. The current image transformation schema and prompt are
not consistently aligned on JSON versus XML, so consumers must not assume a
stable inner format until this is resolved.

## `POST /api/webtorecipe/`

Imports a recipe from a website URL.

```json
{
  "website": "https://recipes.example.invalid/pasta"
}
```

**Response (`200`):**

```json
{
  "ocrtext": "<synthetic-import-result>"
}
```

**Response (`400`):** URL validation error. **Data effects:** the Backend calls
Groq with its website tool enabled. No recognizable server-side SSRF protection,
domain allowlist, size limit, or timeout policy is visible; treat external URLs
as untrusted.

## `POST /api/sharerecipe/`

Creates a public snapshot for a recipe owned by the current user.

```json
{
  "id": 42
}
```

**Response (`200`):**

```json
{
  "success": "<synthetic-share-secret>"
}
```

**Response (`404`):** recipe not found or access denied. **Response (`400`):**
invalid ID. **Data effects:** generates a random URL-safe secret and copies the
recipe user, title, body, and image into `RecipeShare`.

The current model has no visible expiry, revocation flag, source recipe
relationship, or delete endpoint.

## `GET /api/getsharerecipe/`

Renders the public shared-recipe HTML page. The request is unauthenticated and
requires a query parameter:

```text
/api/getsharerecipe/?secret=<synthetic-share-secret>
```

**Response:** rendered HTML. An unknown or missing recipe results in a page with
fallback metadata rather than the JSON errors used by the data endpoint.

## `GET /api/getsharerecipe/data/`

Returns data for a public share secret.

```text
/api/getsharerecipe/data/?secret=<synthetic-share-secret>
```

**Response (`200`):**

```json
{
  "title": "Pasta",
  "body": "{\"recipe_title\":\"Pasta\",\"ingredients\":[],\"instructions\":[]}",
  "image": "recipes/pasta.jpg"
}
```

**Response (`400`):** `secret` is missing. **Response (`404`):** secret is not
valid. **Data effects:** public read of the stored snapshot; no user ownership
check is performed because the secret is the access capability.

## Open questions

- Are the model field names (`body`, `categories`) the stable public contract?
- What is the versioned inner recipe format and AI response schema?
- What limits and security controls apply to image and website inputs?
- What expiry and revocation semantics apply to public share secrets?

## Further reading

- [API overview](../api-overview.md)
- [Backend data model](../../04-backend/data-model.md)
- [Category endpoints](categories.md)
- [Troubleshooting](../../02-getting-started/troubleshooting.md)

## Change history

| Date | Change |
|---|---|
| 2026-09-02 | Initial grouped recipe, AI, and sharing endpoint reference |
