# Dokumentationsplan für Just Cook

| Feld | Wert |
|---|---|
| Status | Entwurf auf Basis des Iststands |
| Erstellt | 2026-09-02 |
| Geltungsbereich | `Just_Cook_Index`, `Just_Cook_Frontend`, `Just_Cook_Backend`, `Just_Cook_Auth` |
| Verbindliche Quelle dieses Dokuments | noch nicht festgelegt |
| Zielgruppe | Entwicklung, Betrieb, Security, Produkt, Support und rechtliche Verantwortliche |

## 1. Ziel

Die spätere Dokumentation soll eine einzige nachvollziehbare Beschreibung des
gesamten Systems liefern. Sie soll nicht nur erklären, wie einzelne Dateien
funktionieren, sondern vor allem diese Fragen beantworten:

- Welche Komponente ist wofür verantwortlich?
- Welche Domain, Umgebung und Datenbank gehören zusammen?
- Wie läuft ein Benutzer von Registrierung bis zum gespeicherten Rezept?
- Welche API-, Auth-, Daten- und Bildformate sind verbindlich?
- Wie werden KI, E-Mail, OAuth, Storage und Sharing betrieben?
- Welche Sicherheits-, Datenschutz- und Verfügbarkeitsannahmen gelten?
- Was ist implementiert, was nur geplant und was bereits veraltet?
- Wie werden Deployment, Fehler, Migration, Rotation und Rollback durchgeführt?

## 2. Dokumentationsprinzipien

### 2.1 Eine Quelle pro Vertrag

Produktregeln, API-Schemas, JWT-Claims, Umgebungswerte und Datenmodelle dürfen
nicht in mehreren Dateien unabhängig gepflegt werden.

- Produkt- und Tarifregeln gehören in die zentrale Produktdokumentation.
- API-Request- und Response-Schemas gehören in die zentrale API-Referenz.
- JWT- und JWKS-Regeln gehören in den Auth-Vertrag.
- Tabellen, Identitäten und Löschregeln gehören in das Datenmodell.
- Werte pro Umgebung gehören in die Environment-Matrix.
- Repository-READMEs enthalten nur Quickstart und Links auf diese Quellen.

### 2.2 Ist und Soll strikt trennen

Jede Aussage über Verhalten wird als `IST`, `SOLL`, `OFFEN`, `VERALTET` oder
`RISIKO` markiert. Ein geplanter Fix darf nicht stillschweigend als bereits
implementiert beschrieben werden.

Beispiel:

```text
IST: Das Backend akzeptiert derzeit den Algorithmus aus dem ungeprüften JWT-Header.
SOLL: Das Backend akzeptiert nur den beschlossenen Algorithmus und prüft Issuer und Audience.
OFFEN: Der verbindliche Algorithmus und die Audience sind noch freizugeben.
```

### 2.3 Jede technische Aussage braucht einen Nachweis

Ein Dokument muss bei wichtigen Aussagen mindestens eine Quelle nennen:

- Repository und relativer Dateipfad
- Commit oder verifizierter Betriebsstand
- bei Code möglichst Zeilenbereich
- bei externen Einstellungen zuständiges System und Prüfdatum

Wenn eine Aussage nur aus statischer Analyse stammt, muss das kenntlich sein.

### 2.4 Keine Geheimnisse dokumentieren

Niemals in Markdown, Beispielpayloads, Screenshots, Logs oder Diagrammen
ablegen:

- API-Keys
- Datenbankpasswörter
- OAuth-Secrets
- Better-Auth-Secrets
- Resend-Keys
- Bearer-Tokens
- Thumbor-Secrets
- private JWKS-Schlüssel

Stattdessen werden Variablenname, Zweck, Bezugsquelle, Rotation und benötigte
Berechtigung beschrieben. Beispielwerte müssen eindeutig synthetisch sein.

### 2.5 Beispiele müssen ausführbar oder ausdrücklich illustrativ sein

Ein API-Beispiel erhält die Kennzeichnung `Beispiel` oder `ausführbarer
Smoke-Test`. Veraltete Beispiele dürfen nicht neben aktuellen Beispielen ohne
Warnung stehen.

### 2.6 Diagramme zeigen Vertrauensgrenzen

System- und Sequenzdiagramme müssen Browser, öffentliche Website, Frontend,
Auth-Service, Django-API, Datenbank, Bilddienst, KI-Dienste, OAuth-Provider
und E-Mail-Dienst unterscheiden. Datenflüsse mit personenbezogenen Rezept-
oder Bilddaten werden sichtbar markiert.

## 3. Pflichtmetadaten für jedes spätere Dokument

Jede Datei unter dem späteren `docs/`-Ordner beginnt mit einer kurzen
Metadatentabelle:

| Feld | Inhalt |
|---|---|
| Status | `IST`, `SOLL`, `OFFEN`, `VERALTET` oder Kombination |
| Verantwortlich | Person oder Rolle, nicht nur ein Repository |
| Zielgruppe | Lesergruppe |
| Letzte Prüfung | Datum und geprüfter Commit beziehungsweise Betriebsstand |
| Quellen | Repositories, Dateien, Tickets oder externe Systeme |
| Abhängigkeiten | Verlinkte Dokumente, die zuerst aktualisiert werden müssen |
| Nächste Prüfung | Datum oder auslösendes Ereignis |

## 4. Empfohlene Benennung und Sprache

- Dokumentation und Erklärtexte werden auf Deutsch geschrieben.
- Codeelemente, Endpunkte, Feldnamen und externe Produktnamen bleiben exakt in
  ihrer technischen Schreibweise.
- Dateinamen werden kleingeschrieben und mit Bindestrichen getrennt.
- Fachbegriffe erhalten im Glossar eine eindeutige Definition.
- `Cooklify` und `Just Cook` werden nicht synonym verwendet, ohne die
  historische beziehungsweise aktuelle Bedeutung zu erklären.
- Zeitangaben werden als UTC oder mit expliziter Zeitzone angegeben.
- Geldbeträge enthalten Währung und Gültigkeitsdatum.

## 5. Zielordner

Die geplanten fertigen Inhalte liegen später unter:

```text
Just_Cook/
├── README.md
├── CONTRIBUTING.md
├── CHANGELOG.md
├── LICENSE
├── docs/
│   ├── README.md
│   ├── 01-overview/
│   ├── 02-getting-started/
│   ├── 03-auth/
│   ├── 04-backend/
│   ├── 05-frontend/
│   ├── 06-index/
│   ├── 07-api/
│   ├── 08-deployment/
│   └── 09-development/
├── Just_Cook_Auth/
├── Just_Cook_Backend/
├── Just_Cook_Frontend/
└── Just_Cook_Index/
```

Die Dateien in `Just_Cook_Docs` sind der Plan für diesen Baum. Der vollständige
Dateibaum und die Inhalte jeder Datei stehen in `ZIELSTRUKTUR.md`.

## 6. Inhaltliche Ebenen

### Ebene A: Orientierung

Leser müssen in wenigen Minuten Systemgrenzen, Zielgruppen, Repositories,
Domains und den Einstiegspunkt verstehen. Diese Ebene enthält keine
Implementierungsdetails, die nur in Quellcodequellen belegt sind.

### Ebene B: Produkt und Nutzer

Diese Ebene beschreibt tatsächliche Nutzerflüsse und den Status von Features.
Marketingtexte, geplante Preise und technische Verfügbarkeit werden separat
ausgewiesen.

### Ebene C: Verträge

Diese Ebene definiert API, Datenformate, Authentifizierung, Fehler, Statuscodes,
Bildpfade, KI-Antworten und externe Integrationen. Sie ist die wichtigste
gemeinsame Referenz für Frontend, Backend und Auth-Service.

### Ebene D: Betrieb und Schutz

Diese Ebene beschreibt Umgebungen, Deployment, Migrationen, Monitoring,
Backups, Secrets, Schlüssel, Datenschutz, Incidents und Rollback.

### Ebene E: Qualität und Historie

Diese Ebene enthält Teststrategie, Abnahmekriterien, Release-Checklisten,
Entscheidungen und klar gekennzeichnete Legacy-Inhalte.

## 7. Was in Repository-READMEs stehen soll

Jedes Repository soll später ein kurzes eigenes `README.md` bekommen. Es
enthält ausschließlich:

- Zweck und Verantwortungsgrenze des Repositories
- Voraussetzungen und lokaler Quickstart
- relevante Start-, Build-, Test- und Docker-Befehle
- benötigte Variablennamen ohne Secretwerte
- lokale Standardports
- Verweis auf den zentralen Systemkontext
- Verweis auf die zuständige zentrale API-, Auth- oder Betriebsdokumentation
- bekannte Blocker und bekannte Abweichungen vom Sollvertrag

Es enthält ausdrücklich nicht erneut die vollständige API, die komplette
Security-Policy, Produktpreise oder dieselben Auth-Flows in einer zweiten
Version.

## 8. Mindestinhalt wichtiger Dokumenttypen

### Systemdokument

Verantwortungsgrenzen, Komponenten, Domains, Vertrauensgrenzen, Hauptdaten-
flüsse, externe Dienste, Umgebungen und nicht vorhandene Komponenten.

### Produkt- und Flowdokument

Zielgruppe, Vorbedingungen, Primärfluss, alternative Flüsse, Fehlerfälle,
Datenänderungen, sichtbare Rückmeldung und aktueller Implementierungsstatus.

### API-Dokument

Methode, Pfad, Authentifizierung, Header, Request-Schema, Response-Schema,
Statuscodes, Fehlerkörper, Berechtigungen, Nebenwirkungen, Limits, Idempotenz,
Beispiele, Tests und Deprecation-Regel.

### Datenmodelldokument

Tabellen oder Objekte, Feldtypen, Pflichtfelder, Beziehungen, Besitz,
Lebenszyklus, Löschung, Aufbewahrung, Migrationen, Indizes und bekannte
Inkonsistenzen.

### Runbook

Voraussetzungen, Sicherheitswarnung, Schrittfolge, erwartete Ergebnisse,
Validierung, Rollback, Logs, Eskalation und Nachbereitung.

### Entscheidungsdokument

Kontext, Problem, Optionen, Entscheidung, Konsequenzen, Alternativen,
betroffene Dokumente und Auslöser für eine Neubewertung.

## 9. Bestehende Dokumentation behandeln

Die bestehenden Dokumente werden nicht kommentarlos gelöscht. Stattdessen:

| Quelle | Geplanter Umgang |
|---|---|
| Frontend `better-auth-sso.md` | Als aktuelle Ausgangsquelle prüfen und in den zentralen Auth-Vertrag überführen |
| Frontend `architecture.md` | Als veraltet markieren oder archivieren; Datei ist beschädigt und beschreibt nicht vorhandene Pfade |
| Frontend `refactor.md` | Als historischer Plan archivieren |
| Frontend `api-standardization-plan.md` | Historischen Plan vom freigegebenen API-Vertrag trennen |
| Frontend `coding-style.md` und `Notes.md` | Gegen den aktuellen Code prüfen, danach aktualisieren oder archivieren |
| Backend `Introduction.md` | Nicht als Quickstart verwenden; falsche Pfade, Befehle und Portangaben ersetzen |
| Backend `Important Classes.md` | Als veraltete Legacy-Dokumentation markieren; enthält alte Auth- und Secret-Beispiele |
| Backend `docs/docs/Backend/requirements.txt` | Nicht als Dependency-Quelle verwenden, bis die verbindliche Quelle entschieden ist |
| leeres Backend-`README.md` | Nach Klärung von Runtime und Dependency-Quelle neu schreiben |

## 10. Pflegeprozess

Eine Dokumentänderung ist bei diesen Ereignissen verpflichtend:

- Änderung eines API-Pfads, Feldnamens, Statuscodes oder Fehlerformats
- Änderung an JWT-Claims, Algorithmus, Issuer, Audience oder TTL
- Änderung an Domain, CORS, Cookie, OAuth-Callback oder Environment-Variable
- Änderung an Datenbankmodell, Migration, Lösch- oder Aufbewahrungsregel
- Änderung an KI-Modell, Prompt, Inputformat, Outputformat oder Kosten
- Änderung an Tarif, Creditverbrauch, Entitlement oder AGB
- Änderung am Dockerfile, Package Manager, Runtime oder Reverse Proxy
- Änderung an externer Datenverarbeitung oder Drittanbieter
- Security Incident, Secret-Rotation oder Schlüsselrotation

Der Pull Request oder das Release muss dann die betroffenen Dokument-IDs
nennen. Ein Dokument gilt erst als aktualisiert, wenn Beispiel, Diagramm,
Testreferenz und Status geprüft wurden.

## 11. Qualitäts-Gate vor Freigabe

Vor der Freigabe des vollständigen Dokumentationssatzes müssen folgende
Fragen mit `Ja` beantwortet werden:

- Gibt es für jede produktive Domain genau eine beschlossene Umgebung?
- Sind Frontend, Backend und Auth auf dieselbe Umgebungsmatrix bezogen?
- Sind API-Feldnamen und KI-Ausgabeformate durch einen gemeinsamen Vertrag gedeckt?
- Sind die aktuellen Auth-Flows inklusive Verifikation, Reset und Logout beschrieben?
- Sind Better-Auth-Tabellen und Django-Tabellen klar unterschieden?
- Sind Migration, Backup, Restore und Rollback ausführbar beschrieben?
- Werden keine Secrets oder echten Tokens veröffentlicht?
- Sind bestehende veraltete Dokumente sichtbar markiert?
- Hat jeder kritische Vertrag mindestens einen automatisierten oder manuellen Testnachweis?
- Sind offene Entscheidungen und technische Risiken im Dokument selbst sichtbar?
