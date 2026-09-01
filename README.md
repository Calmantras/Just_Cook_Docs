# Just Cook Dokumentationsplan

Dieser Ordner enthält den Plan für eine zentrale, repositoryübergreifende
Dokumentation von Just Cook. Er ist zunächst eine Planungs- und Analyseebene,
nicht die fertige Produktdokumentation.

## Warum eine zentrale Dokumentation?

Just Cook besteht aktuell aus vier getrennten Repositories:

| Repository | Tatsächliche Rolle |
|---|---|
| `Just_Cook_Index` | Statische öffentliche Landingpage, Legal-Seite und 404-Seite |
| `Just_Cook_Frontend` | Vue-/Ionic-Anwendung, PWA-/Capacitor-Integration und Browser-Client |
| `Just_Cook_Backend` | Django-/DRF-API für Rezepte, Kategorien, Sharing und KI-Flows |
| `Just_Cook_Auth` | Express-/Better-Auth-Service für Accounts, Sessions und JWTs |

Die wichtigsten Verträge verlaufen über Repositorygrenzen hinweg. Dazu gehören
Domains, Authentifizierung, JWT/JWKS, Rezept- und Kategorieformate, Bildpfade,
KI-Antworten, Sharing, Deployment und Datenschutz. Die bisher vorhandenen
Einzeldokumente enthalten dagegen teilweise alte Dateipfade, alte Authentifi-
zierung und alte API-Formate.

## Einstieg

1. `DOKUMENTATIONSPLAN.md` beschreibt Regeln, Ziel und Verantwortungsgrenzen.
2. `IST-ANALYSE.md` hält den beim Lesen vorgefundenen technischen Stand fest.
3. `ZIELSTRUKTUR.md` übernimmt die vorgegebene Struktur `01-overview` bis `09-development` und beschreibt jede geplante Dokumentdatei.
4. `SCHNITTSTELLEN-REGISTER.md` sammelt die aktuellen, noch nicht überall konsistenten Verträge.
5. `OFFENE-ENTSCHEIDUNGEN.md` listet Entscheidungen, die vor einer verbindlichen Dokumentation geklärt werden müssen.
6. `UMSETZUNGSPLAN.md` legt die Reihenfolge und Abnahmekriterien fest.
7. `vorlagen/` enthält Vorlagen für spätere System-, API-, Runbook- und Entscheidungsdokumente.

## Analysegrundlage

Die Analyse wurde am 2. September 2026 aus dem vorhandenen Arbeitsstand
erstellt. Die vier Repositories sind jeweils eigenständige Git-Repositories;
der übergeordnete Ordner ist kein gemeinsames Git-Repository.

| Repository | Branch | letzter Commit | Arbeitsbaum beim Lesen |
|---|---|---|---|
| `Just_Cook_Auth` | `main` | `1d50168` | sauber |
| `Just_Cook_Backend` | `master` | `220c4fd` | bereits vorhandene Änderungen an `.env`, `requirements.txt`, `token_manager.py`, `settings.py` und ein untracked `.dockerignore` |
| `Just_Cook_Frontend` | `feat/clickable-empty-recipe-icon` | `9a56425` | sauber |
| `Just_Cook_Index` | `switch-to-german` | `70ee732` | sauber |

Die vorhandenen Änderungen im Backend wurden nicht verändert, bereinigt oder
bewertet als eigene Änderungen. Secret-Werte, Tokens und Zugangsdaten werden
in diesem Ordner absichtlich nicht wiedergegeben.

## Statusmodell

Jedes spätere Dokument muss seinen Status eindeutig tragen:

| Status | Bedeutung |
|---|---|
| `IST` | Durch den aktuellen Code oder eine verifizierte Betriebsquelle belegt |
| `SOLL` | Beschlossene Zielarchitektur oder gewünschtes Verhalten |
| `OFFEN` | Entscheidung oder Nachweis fehlt |
| `VERALTET` | Historischer Inhalt, darf nicht als aktueller Vertrag verwendet werden |
| `RISIKO` | Beobachtung, die vor einem produktiven Betrieb bewertet werden muss |

## Wichtige Abgrenzung

Die geplante Dokumentation darf Marketingaussagen nicht als technische
Funktionalität ausgeben. Die Landingpage bewirbt beispielsweise OCR, URL-
Import, Sharing und zukünftige Credits; implementiert werden diese Funktionen
in anderen Repositories oder teilweise noch gar nicht. Jede Aussage muss daher
als Produktversprechen, aktueller Codezustand oder geplante Funktion
gekennzeichnet werden.
