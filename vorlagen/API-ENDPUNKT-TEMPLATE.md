# `METHOD /path`

| Field | Value |
|---|---|
| Status | `CURRENT` / `PLANNED` / `OPEN` / `OUTDATED` |
| Authentication | public / Bearer JWT / session |
| Source | View, serializer, or frontend request |

## Purpose

Short description of the endpoint.

## Request

```http
Content-Type: application/json
Authorization: Bearer <synthetic-token>
```

```json
{}
```

Describe required fields and important restrictions briefly. Never use real
tokens or secrets.

## Response

```json
{}
```

List the important success and error status codes.

## Data Effects

Which data is read or changed? Is ownership checked? Is an external service
called?

## Open Questions

Only include this section when the current contract is not clear.
