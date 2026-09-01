# Runbook: Störung oder Betriebsvorgang

| Feld | Wert |
|---|---|
| Status | `SOLL` |
| Verantwortlich | On-call-Rolle |
| Risiko | niedrig / mittel / hoch / kritisch |
| Gültigkeit | Umgebung und Service |
| Letzte Prüfung | YYYY-MM-DD |

## Zweck und Auslöser

Wann wird dieses Runbook verwendet? Welche Symptome oder Alerts lösen es aus?

## Voraussetzungen

Berechtigungen, Werkzeuge, sichere Variablennamen und relevante Dashboards.
Keine echten Zugangsdaten eintragen.

## Sicherheitswarnungen

Auswirkungen auf Benutzer, Daten, Tokens, Migrationen und externe Kosten.

## Diagnose

1. Dienst und Umgebung prüfen.
2. Healthcheck und relevante Logs ohne Secretwerte prüfen.
3. Abhängige Dienste und aktuelle Deployments prüfen.
4. Ursache und Zeitpunkt dokumentieren.

## Maßnahmen

Schrittfolge mit erwarteten Ergebnissen. Für jeden destruktiven Schritt muss
eine Backup- oder Bestätigungsschwelle angegeben werden.

## Verifikation

Welche Requests, Metriken und Nutzerflüsse müssen nach der Maßnahme erfolgreich
sein?

## Rollback und Eskalation

Rollbackschritte, Abbruchkriterien, Verantwortliche und Eskalationskanal.

## Nachbereitung

Incident-ID, Root Cause, Dokumentationsänderung, Test und dauerhafte Maßnahme.
