# `METHOD /path`

| Feld | Wert |
|---|---|
| Status | `IST` / `SOLL` / `VERALTET` |
| Authentifizierung | öffentlich / Bearer-JWT / Session / Admin |
| Verantwortlich | Rolle oder Service |
| Quellen | Backend-View, Serializer, Frontend-Request |
| Version | API-Version oder `nicht versioniert` |
| Idempotenz | ja / nein / unbekannt |

## Zweck

Kurze fachliche Beschreibung.

## Request

### Header

```http
Content-Type: application/json
Authorization: Bearer <synthetischer-token>
```

### Query-Parameter

| Name | Typ | Pflicht | Beschreibung |
|---|---|---|---|
| `name` | `string` | ja/nein | Beschreibung |

### Body

```json
{}
```

Feldregeln, Größenlimits, erlaubte Werte und sensible Daten hier beschreiben.

## Response

### Erfolgsfall

```json
{}
```

### Statuscodes

| Status | Bedeutung | Responsekörper |
|---|---|---|
| `200` | Erfolg | Schema |
| `400` | Eingabefehler | Fehler-Schema |

## Berechtigung und Datenwirkung

Besitzprüfung, Rollen, Tabellenänderung, externe Aufrufe, Nebenwirkungen,
Aufbewahrung und Rollback.

## Fehler, Limits und Observability

Timeouts, Rate-Limits, Retries, Logfelder ohne Geheimnisse, Metriken und
Benutzerfeedback.

## Testnachweis

Verweise auf Unit-, Integration-, Contract- und E2E-Tests.
