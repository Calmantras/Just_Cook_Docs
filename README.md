# Just Cook Documentation

Technical documentation for the four Just Cook repositories. The site is built
from Markdown with [Zensical](https://zensical.org/) and is also intended to be
read directly on GitHub.

The documentation is a practical guide for developers and people who operate
or change the project. It describes the current analyzed state, not an idealized
architecture. Unverified or conflicting facts are explicitly marked `OPEN`.

## Start here

Read the [documentation home](docs/README.md), then continue with:

1. [Project overview](docs/01-overview/project-overview.md)
2. [Prerequisites](docs/02-getting-started/prerequisites.md)
3. [Local development setup](docs/02-getting-started/local-development-setup.md)
4. [API overview](docs/07-api/api-overview.md)
5. [Troubleshooting](docs/02-getting-started/troubleshooting.md)

## Repositories

| Repository | Responsibility |
|---|---|
| `Just_Cook_Index` | Static public landing and legal pages |
| `Just_Cook_Frontend` | Vue/Ionic single-page application and browser client |
| `Just_Cook_Backend` | Django/DRF API for recipes, categories, sharing, and AI features |
| `Just_Cook_Auth` | Better Auth service for accounts, sessions, OAuth, and JWTs |

The four application repositories are independent and remain unchanged by this
documentation project. Source links in the pages point to their neighboring
directories using relative paths.

## Documentation status

The current-state inventory was reviewed on `2026-09-02`. No application or
documentation build is implied by that review. The local setup and deployment
pages intentionally remain a structured skeleton where the source state does
not establish a safe, runnable procedure.

## Contributing documentation

- Keep pages short and link to the source of important technical claims.
- Use `CURRENT`, `PLANNED`, `OPEN`, or `OUTDATED` when the distinction matters.
- Use synthetic values such as `<example-token>`; never add credentials, tokens,
  private keys, or real environment values.
- Update the affected page when an endpoint, environment variable, start command,
  data shape, authentication flow, or deployment step changes.
- Do not treat this documentation as a replacement for the source code.

Zensical configuration and navigation are in [`zensical.toml`](zensical.toml).
The reusable authoring templates remain in [`vorlagen/`](vorlagen/).
the starting point and mark the unresolved issue as `OPEN`.
