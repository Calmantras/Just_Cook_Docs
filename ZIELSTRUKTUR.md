# Zielstruktur der technischen Just-Cook-Dokumentation

| Feld | Wert |
|---|---|
| Status | `SOLL` / vom Projekt vorgegebene Zielstruktur |
| Zielwurzel | `Just_Cook/` |
| Zentrale Dokumentation | `Just_Cook/docs/` |
| Sprache | Deutsch; technische Namen bleiben unverändert |
| Grundlage | Ist-Analyse vom 2026-09-02 |

Diese Struktur ist verbindlich für die geplante technische Dokumentation. Die
bisherigen Planungsbereiche wurden in diese Nummerierung überführt. Ein
Dokument darf nur tatsächliche Funktionen als `IST` beschreiben; reservierte
oder geplante Bereiche werden darin deutlich als `OFFEN` oder `SOLL` markiert.

## 1. Zielbaum

```text
Just_Cook/
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENSE
│
├── docs/
│   ├── README.md
│   │
│   ├── 01-overview/
│   │   ├── project-overview.md
│   │   ├── system-architecture.md
│   │   ├── technology-stack.md
│   │   ├── glossary.md
│   │   └── repository-structure.md
│   │
│   ├── 02-getting-started/
│   │   ├── prerequisites.md
│   │   ├── local-development-setup.md
│   │   ├── environment-variables.md
│   │   ├── running-the-project.md
│   │   └── troubleshooting.md
│   │
│   ├── 03-auth/
│   │   ├── overview.md
│   │   ├── authentication-flow.md
│   │   ├── authorization-and-roles.md
│   │   ├── jwt-or-session-handling.md
│   │   ├── api-reference.md
│   │   └── security.md
│   │
│   ├── 04-backend/
│   │   ├── overview.md
│   │   ├── architecture.md
│   │   ├── api-reference.md
│   │   ├── database.md
│   │   ├── data-models.md
│   │   ├── error-handling.md
│   │   ├── validation.md
│   │   └── testing.md
│   │
│   ├── 05-frontend/
│   │   ├── overview.md
│   │   ├── architecture.md
│   │   ├── pages-and-routing.md
│   │   ├── components.md
│   │   ├── state-management.md
│   │   ├── api-integration.md
│   │   ├── styling-and-design-system.md
│   │   └── testing.md
│   │
│   ├── 06-index/
│   │   ├── overview.md
│   │   ├── responsibilities.md
│   │   ├── integration.md
│   │   └── configuration.md
│   │
│   ├── 07-api/
│   │   ├── api-overview.md
│   │   ├── authentication.md
│   │   ├── endpoints/
│   │   │   ├── auth.md
│   │   │   ├── users.md
│   │   │   ├── recipes.md
│   │   │   ├── ingredients.md
│   │   │   └── categories.md
│   │   ├── request-response-examples.md
│   │   └── error-codes.md
│   │
│   ├── 08-deployment/
│   │   ├── environments.md
│   │   ├── deployment-guide.md
│   │   ├── docker.md
│   │   ├── ci-cd.md
│   │   ├── monitoring-and-logging.md
│   │   └── backup-and-recovery.md
│   │
│   └── 09-development/
│       ├── coding-guidelines.md
│       ├── git-workflow.md
│       ├── branching-strategy.md
│       ├── commit-conventions.md
│       ├── pull-request-guidelines.md
│       └── testing-strategy.md
│
├── Just_Cook_Auth/
│   └── README.md
├── Just_Cook_Backend/
│   └── README.md
├── Just_Cook_Frontend/
│   └── README.md
└── Just_Cook_Index/
    └── README.md
```

`Just_Cook_Docs` bleibt bis zu einer möglichen Zusammenführung die
Planungswurzel. Der obige Baum beschreibt die spätere kanonische Struktur
unter einer gemeinsamen Projektwurzel `Just_Cook/`.

## 2. Dateien in der Projektwurzel

| Datei | Verbindlicher Inhalt | Aktueller Status |
|---|---|---|
| `README.md` | Produktzweck, Systemkarte, Repositorykarte, Quickstart, zentrale Links, bekannte P0-Blocker | `SOLL` |
| `CONTRIBUTING.md` | Entwicklungsumgebung, Dokumentationspflicht, Testpflicht, Security-Meldeweg, PR-Ablauf | `SOLL` |
| `CHANGELOG.md` | versionierte, nutzer- und betriebsrelevante Änderungen; kein Ersatz für Git-Log | `SOLL` |
| `LICENSE` | für die gemeinsame Projektdokumentation und den Quellcode gültige Lizenz | `OFFEN`: Backend deklariert aktuell eine eigene CC-BY-NC-ND-Referenz, eine Gesamtentscheidung fehlt |

## 3. `docs/01-overview/`

| Datei | Muss enthalten |
|---|---|
| `project-overview.md` | Produktziel, Zielgruppen, tatsächliche Features, geplante Features, Nicht-Ziele und Abgrenzung von Marketingclaims |
| `system-architecture.md` | C4-Kontext, Komponenten, Datenflüsse, Trust Boundaries, Domains, externe Dienste, öffentliche Share-Seite und Django-Admin |
| `technology-stack.md` | Vue/Ionic/Capacitor/Vite, Django/DRF/Gunicorn, Express/Better Auth, PostgreSQL, Docker/Nginx, Thumbor, Cerebras, Groq, Resend, Google und Discord; Versionen und Quellen |
| `glossary.md` | Just Cook/Cooklify, Recipe, RecipeBody, Session, JWT, JWKS, Share Secret, OCR, Credit, PWA, Django Admin, `id` und `sub` |
| `repository-structure.md` | Verantwortung, Startpunkt, Laufzeit, Port, Buildartefakt, Datenbesitz und README je Repository |

## 4. `docs/02-getting-started/`

| Datei | Muss enthalten |
|---|---|
| `prerequisites.md` | verbindliche Betriebssystem-, Docker-, Node-/npm- oder Bun-, Python- und PostgreSQL-Voraussetzungen; aktuelle Konflikte klar markieren |
| `local-development-setup.md` | sichere lokale Konfiguration aller vier Repositories, Reihenfolge der Services, Migrationen und Testdaten |
| `environment-variables.md` | Variablenname, Zweck, Pflicht, Beispielwert ohne Geheimnis, Besitzer, Verbrauchsstelle, Umgebung und Rotation; Auth-, Backend- und Frontendwerte getrennt |
| `running-the-project.md` | konkrete Start-, Build-, Lint-, Test- und Stop-Befehle nach Entscheidung über Runtime und Paketmanager |
| `troubleshooting.md` | CORS, `localhost` gegen `127.0.0.1`, OAuth-State, fehlendes `src/env.ts`, JWKS, Migration, Bilddienst, KI, Legal-Loader und PWA-Cache |

## 5. `docs/03-auth/`

| Datei | Muss enthalten |
|---|---|
| `overview.md` | Better-Auth-Service, Express-Mount, PostgreSQL, Resend, OAuth-Provider, Session-Cookies und Beziehung zu Frontend/Backend |
| `authentication-flow.md` | E-Mail-Registrierung, Verifikation, Resend, Login, Google/Discord, Passwort-Reset, Session, `/token` und Logout inklusive Fehlerfälle |
| `authorization-and-roles.md` | aktuelles Besitzmodell anhand der Benutzer-ID, fehlende Rollen, Trennung zum Django-Admin, Account-Löschung und offene Autorisierungsentscheidungen |
| `jwt-or-session-handling.md` | Cookie- und JWT-Lebenszyklus, JWKS, Claims, Algorithmus, Issuer, Audience, TTL, Cache, 401, Logout und Revocation; Ist/Soll getrennt |
| `api-reference.md` | relevante Better-Auth-Routen, Redirects, Cookies, Statuscodes und Better-Auth-Version; generierte Routen als versionsabhängig markieren |
| `security.md` | CORS, Cookies, OAuth-State, `localStorage`, Rate Limits, Secretrotation, Token-Diebstahl, Schlüsselrotation und Threat Model |

## 6. `docs/04-backend/`

| Datei | Muss enthalten |
|---|---|
| `overview.md` | Verantwortung der Django-API, Abgrenzung zu Auth, Index und Frontend sowie öffentliche Share-Seite |
| `architecture.md` | `src`-Projektstruktur, URLs, Views, Serializer, Middleware, Django-Admin, KI- und Bildintegrationen |
| `api-reference.md` | Ist- und Sollvertrag für alle Django-Endpunkte; Methode, Auth, Schema, Status, Fehler, Ownership, Nebenwirkung und Limit |
| `database.md` | PostgreSQL, Rollen, Auth- und Domänenschema, Migrationen, Indizes, Backup, Restore und Grenzen der Shared-DB-Annahme |
| `data-models.md` | `Recipe`, `Category`, `RecipeShare`, Better-Auth-Identität als String, RecipeBody, Kategorien-CSV, Bilder und Share-Snapshot |
| `error-handling.md` | einheitliches Fehlerformat und Statussemantik; aktuelle falsche oder fehlende Fehlerfälle transparent markieren |
| `validation.md` | JWT-Claims, Requestfelder, JSON-Rezeptkörper, Kategorien, Base64/MIME/Größe, URL-Allowlist, SSRF, Prompt Injection und externe Timeouts |
| `testing.md` | Unit-, API-, Datenbank-, Contract- und Integrationsstrategie; Mocks für Thumbor, Cerebras und Groq |

## 7. `docs/05-frontend/`

| Datei | Muss enthalten |
|---|---|
| `overview.md` | Vue-/Ionic-App, PWA-/Capacitor-Umfang, Nutzerfunktionen und Grenzen zu Backend/Auth |
| `architecture.md` | Einstieg, Lazy Routes, Request-Layer, globale Services, CustomEvents, lokaler Cache, PWA und externe Browserassets |
| `pages-and-routing.md` | jede Route, Public/Protected-Regel, Queryparameter, Navigation, Lade-, Leer- und Fehlerzustände |
| `components.md` | gemeinsame Vue-Komponenten, Ionic-Komponenten, Verantwortlichkeiten, Props/Events und Accessibility-Anforderungen |
| `state-management.md` | `recipes_service`, `categories_service`, Events, Cachelebensdauer, Benutzertrennung, Filter- und Kategorie-Sonderwerte |
| `api-integration.md` | Better-Auth-Client, Bearer-Header, Response-Mapping, Statusauswertung, 401-Verhalten, Bild- und Share-URL-Normalisierung |
| `styling-and-design-system.md` | CSS-Tokens, Layout, Breakpoints, Ionic-Theming, Accessibility, Designquellen und Abgleich mit aktuellem Code |
| `testing.md` | Unit-, Component- und E2E-Tests, API-Mocks, Auth-Flows, Cache-/Benutzerwechsel, mobile Browser und PWA |

## 8. `docs/06-index/`

| Datei | Muss enthalten |
|---|---|
| `overview.md` | Rolle als statische Landingpage statt Produktanwendung, Zielgruppen und Abgrenzung |
| `responsibilities.md` | Marketingcontent, CTAs, Legal-Hülle, 404, Assets, SEO; ausdrücklich keine API, Authentifizierung oder Rezeptverarbeitung |
| `integration.md` | Links zu App-Login/Registrierung, Domainwechsel, Legal-Provider, externe Scripts, Ausfallverhalten und keine belegte SSO-Übergabe |
| `configuration.md` | Nginx-Routen, Redirects, Assetcache, Docker/Compose, Canonical, robots/sitemap, TLS/CDN/Security Header und Content-Freigabe |

## 9. `docs/07-api/`

Dieser Bereich ist die kanonische, repositoryübergreifende API-Referenz. Die
Detailseiten im Backend und Auth-Bereich verweisen darauf und duplizieren den
Vertrag nicht.

| Datei | Muss enthalten | Status im aktuellen Code |
|---|---|---|
| `api-overview.md` | Basis-URLs je Umgebung, Versionierung, Konventionen, Auth, Medien, Pagination, Deprecation und Ownership | P0; mehrere Punkte offen |
| `authentication.md` | Bearer-JWT, Session-Cookie, JWKS, Claims, CORS und 401 | P0; Sollvertrag benötigt |
| `endpoints/auth.md` | Better-Auth-Endpoints, OAuth-Callbacks, Session, Token, JWKS und Health | vorhanden über Better Auth, versionsabhängig |
| `endpoints/users.md` | Profil, Account-Löschung, Rollen und Identitätsreferenz | `OFFEN`: kein eigener User-Endpoint im Django-Backend gefunden |
| `endpoints/recipes.md` | Rezept-CRUD, OCR, OCR-Transformation, Website-Import und Sharing | vorhanden, aber Feld- und KI-Output-Verträge teils widersprüchlich |
| `endpoints/ingredients.md` | eigene Ingredient-Ressource oder begründete Abwesenheit | `OFFEN`: keine eigene Ingredient-Ressource oder kein eigener Endpoint gefunden; Zutaten liegen in `RecipeBody` |
| `endpoints/categories.md` | Kategorien, Favoritenmodell, Sonderwerte und Rezept-Kategoriezuweisung | vorhanden, aber CSV-/Sonderwertmodell und Create-Response klären |
| `request-response-examples.md` | synthetische Beispiele, Header, Statuscodes, Fehler, Bilder und keine Secrets | P0 |
| `error-codes.md` | einheitliche Fehlercodes, HTTP-Status, Nutzertext, Logkontext und Retry-Entscheidung | P0; derzeit nicht einheitlich |

## 10. `docs/08-deployment/`

| Datei | Muss enthalten |
|---|---|
| `environments.md` | Local, Testing und Production: Domains, Ports, Origins, OAuth-Callbacks, Datenbanken, Bildhost, Secrets und Verantwortliche |
| `deployment-guide.md` | Freigabe, Build, Migration, Deploymentreihenfolge, Healthchecks, Smoke-Test, Rollback und Dokumentationsupdate |
| `docker.md` | Dockerfiles, Compose, Image-Pinning, `.dockerignore`, Nutzer, Ports, Buildkontext und Secret-Injektion |
| `ci-cd.md` | gewünschte Pipeline, Checks, Image-Scan, Contract-Tests, Migrationscheck, Staging und Releaseprozess; aktuell kein CI als Ist markieren |
| `monitoring-and-logging.md` | Health, Logs, Metriken, Alerts, Kosten, Datenschutz, Secret-Redaction und externe Dienstüberwachung |
| `backup-and-recovery.md` | PostgreSQL, Better-Auth-Tabellen, Domänendaten, Bildreferenzen, Restore-Test, RPO/RTO und Rollen |

## 11. `docs/09-development/`

| Datei | Muss enthalten |
|---|---|
| `coding-guidelines.md` | Sprache, Typisierung, Error Handling, API-Konventionen, Secrets, Accessibility und Dokumentationsmetadaten |
| `git-workflow.md` | lokale Arbeit, Branch, Reviews, Changelog, keine Secrets, Migrationen und Abgleich über vier Repositories |
| `branching-strategy.md` | Branchtypen, Release- und Hotfix-Ablauf, Synchronisation von Auth-/Frontend-/Backend-Verträgen |
| `commit-conventions.md` | konsistente Committypen, Scope je Repository, Breaking Changes, Migration und Security-Kennzeichnung |
| `pull-request-guidelines.md` | erforderliche Beschreibung, Tests, Dokulinks, Screenshots, Contract-Änderungen, Env-/Migration-/Security-Checkliste |
| `testing-strategy.md` | projektweite Testpyramide, Contract-Tests, E2E, Mocks, Testdaten, CI-Gates und Freigabekriterien |

## 12. Repository-READMEs

| Repository | README-Inhalt |
|---|---|
| `Just_Cook_Auth/README.md` | Zweck, Better Auth, Voraussetzungen, sichere Env-Variablen, Migration, Start/Docker, Healthcheck, Tests und Links zu `03-auth`/`08-deployment` |
| `Just_Cook_Backend/README.md` | Django-API-Zweck, Runtime, Dependencyquelle, Env, Migration, Start/Docker, Tests, Admin und Links zu `04-backend`/`07-api` |
| `Just_Cook_Frontend/README.md` | Vue/Ionic-Zweck, Runtime/Paketmanager, Env, Dev/Build/Test, PWA/Capacitor und Links zu `05-frontend`/`07-api` |
| `Just_Cook_Index/README.md` | statische Landingpage, Nginx/Docker, lokale Auslieferung, Routen, Content/Legal/SEO und Links zu `06-index`/`08-deployment` |

Repository-READMEs bleiben Quickstarts. Sie kopieren weder die vollständige
API-Referenz noch den Auth-, Security- oder Deploymentvertrag.

## 13. Explizit nicht als bestehend dokumentieren

- Es gibt aktuell keinen eigenen Django-User-API-Bereich.
- Es gibt aktuell keine eigenständige Ingredient-Ressource.
- Es gibt keine sichtbare Rollen- oder Entitlement-Implementierung.
- Es gibt keine sichtbare Billing-/Credit-Durchsetzung, obwohl Landingpage und
  Subscription-Ansicht Tarife und Credits beschreiben.
- Es gibt keine vollständige CI-Pipeline, Auth-Testabdeckung oder Index-
  Smoke-Teststruktur im aktuellen Stand.
- Es gibt keinen belegten vollständigen nativen Android- oder iOS-Releaseprozess.

Diese Punkte werden nicht aus der Struktur entfernt, sondern in den genannten
Dateien als `OFFEN`, `SOLL` oder absichtlich nicht vorhandene Ressource
dokumentiert.
