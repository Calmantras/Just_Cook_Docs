# Just Cook

| Field | Value |
|---|---|
| Status | `CURRENT` |
| Last reviewed | 2026-09-02 |
| Sources | Reviewed source repositories and the documentation structure in this repository |

Just Cook is a recipe application made up of a public website, a Vue/Ionic
client, a Django API, and a separate Better Auth service. This documentation
explains how those parts fit together and records the interfaces that were
visible in the reviewed source snapshot.

## Choose a path

| If you need to... | Start with... |
|---|---|
| Understand the product boundary | [Project overview](01-overview/project-overview.md) |
| See how requests move through the system | [System architecture](01-overview/system-architecture.md) |
| Prepare a local workspace | [Prerequisites](02-getting-started/prerequisites.md) and [setup skeleton](02-getting-started/local-development-setup.md) |
| Understand sign-in and tokens | [Authentication overview](03-auth/overview.md) |
| Change recipes or categories | [Backend overview](04-backend/overview.md) and the [API overview](07-api/api-overview.md) |
| Change frontend navigation or state | [Routes and state](05-frontend/routes-and-state.md) |
| Investigate a known mismatch | [Troubleshooting](02-getting-started/troubleshooting.md) |
| Deploy a service | [Deployment](08-deployment/deployment.md) |

## Reading the labels

- `CURRENT` means the statement is present in the reviewed code or configuration.
- `PLANNED` means desired behavior that is not documented as implemented.
- `OPEN` means that the source snapshot does not establish the answer or contains
  a contradiction.
- `OUTDATED` identifies historical material that must not be used as a current
  implementation reference.

## Scope and sources

This first version is based on the static inventory reviewed on `2026-09-02`.
The four source repositories are read-only inputs to this site. Important pages
link to concrete source files; use those files as the final authority when the
implementation changes.

The documentation deliberately does not contain real credentials. Pricing,
credits, billing, entitlement enforcement, and the final native-app scope are
`OPEN` in the reviewed state.

## Navigation map

- [Overview](01-overview/project-overview.md): product boundary, architecture,
  and repository responsibilities.
- [Getting started](02-getting-started/prerequisites.md): requirements,
  service-by-service setup skeleton, variables, and known problems.
- [Authentication](03-auth/overview.md): Better Auth, cookies, JWTs, and flows.
- [Backend](04-backend/overview.md): Django, integrations, and data model.
- [Frontend](05-frontend/overview.md): Vue/Ionic routes and client state.
- [Index](06-index/overview.md): landing page, legal page, and Nginx boundary.
- [API](07-api/api-overview.md): active grouped endpoint reference.
- [Deployment](08-deployment/deployment.md): known container behavior and open
  release steps.
- [Development](09-development/coding-guidelines.md): conventions and existing
  test information.

## Maintaining the site

When a start command, environment variable, endpoint, authentication claim,
data shape, external integration, or deployment step changes, update the related
page in the same change. Run static link and secret checks before publishing.
