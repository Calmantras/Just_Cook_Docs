# Umsetzungsplan für die technische Dokumentation

| Feld | Wert |
|---|---|
| Status | `SOLL` / an die vorgegebene Struktur angepasst |
| Zielstruktur | `ZIELSTRUKTUR.md` |
| Grundsatz | Erst Fakten, Verträge und Risiken klären; danach freigegebene Dokumente schreiben |

## Phase 0: Schutz und Dokumentationsbasis

| Aufgabe | Ergebnis | Zielbereich |
|---|---|---|
| Aktuelle und historisch exponierte Secrets rotieren und den Vorfall ohne Werte festhalten | Keine Geheimnisse werden weiterverwendet oder dokumentiert | `03-auth/security.md`, `08-deployment/` |
| Backend-Arbeitsstand und die vorhandenen uncommitted Änderungen als Dokumentationsbasis festhalten | eindeutiger Commit-/Arbeitsbaumbezug | `docs/README.md`, `01-overview/` |
| Veraltete und beschädigte Dokumente markieren oder aus der aktiven Navigation nehmen | keine Legacy-Quelle gilt als aktueller Vertrag | `CHANGELOG.md`, `09-development/` |
| Dokument-Owner für Engineering, Betrieb, Security, Produkt und Legal benennen | klare Reviewverantwortung | `CONTRIBUTING.md` |

## Phase 1: Wurzel und Überblick

| Aufgabe | Ergebnis |
|---|---|
| Projekt-README schreiben | `README.md` mit Produktzweck, Systemkarte, Quickstart, Repositories und zentralen Links |
| Beitrags- und Reviewregeln schreiben | `CONTRIBUTING.md` |
| Changelog- und Lizenzentscheidung festlegen | `CHANGELOG.md`, `LICENSE` |
| zentrale Dokumentnavigation schaffen | `docs/README.md` |
| System, Stack, Begriffe und Repositories beschreiben | vollständiger Bereich `docs/01-overview/` |

**Abnahme:** Neue Mitwirkende erkennen Komponenten, Verantwortlichkeiten,
Umgebungsgrenzen und den richtigen Einstieg ohne Quellcode-Suche.

## Phase 2: Lokale Entwicklung und Umgebungen

| Aufgabe | Ergebnis |
|---|---|
| verbindliche Runtime- und Paketmanagerentscheidung treffen | `02-getting-started/prerequisites.md` |
| Local, Testing und Production vollständig abgleichen | `02-getting-started/environment-variables.md`, `08-deployment/environments.md` |
| sicheren lokalen Mehrservice-Start beschreiben | `02-getting-started/local-development-setup.md`, `running-the-project.md` |
| typische Fehlerfälle aus der Ist-Analyse aufnehmen | `02-getting-started/troubleshooting.md` |
| alle vier Repository-READMEs als Quickstarts anlegen | `Just_Cook_*/README.md` |

**Abnahme:** Ein Entwickler kann eine lokale Umgebung nach beschlossener
Runtime aufbauen, ohne Secretwerte zu benötigen und ohne `localhost` mit
`127.0.0.1` im OAuth-Flow zu vermischen.

## Phase 3: Auth, API und Datenverträge

| Aufgabe | Ergebnis |
|---|---|
| Auth-Architektur, Flows, Besitzmodell, JWT/JWKS und Security beschreiben | vollständiger Bereich `docs/03-auth/` |
| Django-Architektur, DB, Modelle, Validation, Fehler und Tests beschreiben | vollständiger Bereich `docs/04-backend/` |
| kanonische API-Navigation und Fehlerkonvention bestimmen | `docs/07-api/api-overview.md`, `error-codes.md` |
| Auth-, Recipe-, Category- und reservierte User-/Ingredient-Endpunkte erfassen | `docs/07-api/endpoints/` |
| synthetische Request-/Response-Beispiele schreiben | `docs/07-api/request-response-examples.md` |
| Frontend-Mappings gegen den zentralen API-Vertrag abgleichen | `docs/05-frontend/api-integration.md` |

**Abnahme:** Für jeden aktiven Endpunkt gibt es Methode, Auth, Request,
Response, Statuscodes, Fehler, Ownership und einen Testnachweis. `users.md`
und `ingredients.md` dokumentieren ausdrücklich, dass sie aktuell keine
eigenständige Django-Ressource darstellen.

## Phase 4: Frontend und Index

| Aufgabe | Ergebnis |
|---|---|
| Frontend-Architektur, Pages, Komponenten, Services, Cache und Design dokumentieren | vollständiger Bereich `docs/05-frontend/` |
| Cache- und Tokenverhalten als Ist/Soll klar dokumentieren | `05-frontend/state-management.md`, `api-integration.md`, `03-auth/jwt-or-session-handling.md` |
| Landingpage von der eigentlichen App abgrenzen | `docs/06-index/overview.md`, `responsibilities.md` |
| Nginx, Legal-Loader, CTAs, SEO und Contentkonfiguration dokumentieren | `docs/06-index/integration.md`, `configuration.md` |

**Abnahme:** Produkt, Marketing und Engineering unterscheiden sicher zwischen
Landingpage-Claim, implementierter App-Funktion und geplanter Funktion.

## Phase 5: Deployment, Qualität und Entwicklungsprozess

| Aufgabe | Ergebnis |
|---|---|
| Deploymentreihenfolge, Migrationen, Docker, Secrets, Healthchecks und Rollback beschreiben | `docs/08-deployment/deployment-guide.md`, `docker.md` |
| CI/CD, Monitoring, Logs, Alerts, Backup und Recovery festlegen | restlicher Bereich `docs/08-deployment/` |
| projektweite Testpyramide und Contract-Tests definieren | `09-development/testing-strategy.md`, Bereichstests in `03-auth` bis `05-frontend` |
| Coding-, Git-, Branch-, Commit- und PR-Regeln etablieren | vollständiger Bereich `docs/09-development/` |
| Dokumentationspflege in PR-Checkliste aufnehmen | `CONTRIBUTING.md`, `pull-request-guidelines.md` |

**Abnahme:** Ein Release kann mit dokumentiertem Build, Migration, Smoke-Test,
Rollback, Backup-Prüfung und Changelog freigegeben werden.

## Reihenfolge der P0-Dokumente

1. `docs/README.md`
2. `docs/01-overview/system-architecture.md`
3. `docs/02-getting-started/environment-variables.md`
4. `docs/03-auth/jwt-or-session-handling.md`
5. `docs/04-backend/api-reference.md`
6. `docs/04-backend/data-models.md`
7. `docs/05-frontend/api-integration.md`
8. `docs/07-api/api-overview.md`
9. `docs/07-api/error-codes.md`
10. `docs/08-deployment/environments.md`
11. `docs/08-deployment/deployment-guide.md`
12. `docs/09-development/testing-strategy.md`

## Freigabekriterien

Ein Dokument ist erst freigegeben, wenn es Status, Owner, Quellenstand,
betroffene Umgebungen, offene Entscheidungen, Risiken und Testnachweise nennt.
Bei API-, Auth-, Schema-, Environment-, Deployment-, Preis-, Legal- oder
Security-Änderungen müssen die zugehörigen Dokumente im selben Pull Request
oder Release aktualisiert werden.
