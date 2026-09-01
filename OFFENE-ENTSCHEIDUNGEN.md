# Offene Entscheidungen und Klärungen

| Feld | Wert |
|---|---|
| Status | `OFFEN` / Arbeitsregister |
| Zweck | Entscheidungen sichtbar machen, bevor sie als verbindlicher Dokumentationsvertrag erscheinen |
| Verantwortliche | Je Entscheidung noch festzulegen |
| Regel | Keine `SOLL`-Dokumentation freigeben, solange eine P0-Vertragsentscheidung ungeklärt ist |

## Prioritätsstufen

| Priorität | Bedeutung |
|---|---|
| P0 | Sicherheits-, Datenisolations-, Identitäts- oder Produktionsblocker |
| P1 | Vertrags-, Betriebs- oder Release-Risiko, das vor dem nächsten stabilen Release geklärt werden muss |
| P2 | Wartbarkeit, Produktklarheit oder Optimierung ohne unmittelbaren Blocker |

## Entscheidungsregister

| ID | Priorität | Entscheidung | Warum sie benötigt wird | Belege |
|---|---|---|---|---|
| D-001 | P0 | Welche Frontend-, Backend- und Auth-URL gehört in Local, Testing und Production? | Der Backend-Fallback zeigt auf Testing; Auth-Origins mischen Umgebungen; Frontend-URLs sind teils hartcodiert | Backend `src/main/settings.py:162,175-179`; Auth `.env`/`auth.mjs`; Frontend `.env.example`; Index `index.html` |
| D-002 | P0 | Was ist die kanonische Share-Domain und woher kommt `SHARE_URL`? | Backend erlaubt `share.just-cook.net`, die Frontend-Env-Datei fehlt, und der erzeugte Link ist nicht vollständig belegbar | Backend `settings.py:37-38`; Frontend `recipe.vue`, `.env.example`; Share-Template |
| D-003 | P0 | Welche Runtime- und Dependency-Quelle ist verbindlich? | Backend `pyproject.toml` und `requirements.txt` widersprechen sich; Frontend-Docker nutzt Bun ohne `bun.lock`, während `package-lock.json` vorhanden ist | Backend `pyproject.toml`, `requirements.txt`, Dockerfile; Frontend `package.json`, Dockerfile |
| D-004 | P0 | Wer erzeugt, versioniert und deployt Better-Auth-Migrationen? | Auth benötigt Tabellen für User, Account, Session, Verification und JWKS, führt aber keinen Migrationsschritt aus | Auth `src/lib/auth.mjs:83-89`, Dockerfile, Compose |
| D-005 | P0 | Welche JWT-Claims, Algorithmen, Issuer, Audience und TTL sind verbindlich? | Der Backend-Validator übernimmt den Algorithmus aus dem ungeprüften Header und prüft Issuer/Audience nicht | Backend `token_manager.py:18-35`, Auth JWT-Plugin |
| D-006 | P0 | Wie werden Tokens und lokale Rezeptdaten gespeichert und widerrufen? | JWT liegt in `localStorage`; der Rezeptcache ist nicht benutzerbezogen und bleibt beim Logout | Frontend `auth-client.ts:34-69`; `recipes.service.ts:6-24`; `settings.vue` |
| D-007 | P1 | Wie soll Registrierung mit E-Mail-Verifikation und Social Login funktionieren? | Auth verlangt Verifikation, Frontend navigiert dennoch nach `/recipes`; provider-spezifische Verifikation ist offen | Auth `auth.mjs:41-63,71-81`; Frontend `register.vue`, `verify_email.vue` |
| D-008 | P0 | Welche Response-Feldnamen und welches Recipe-Body-Format sind kanonisch? | Einzelrezept nutzt `body`/`categories` im Backend, aber `recipemd`/`category` im Frontend; KI-Prompt und Parser unterscheiden XML/JSON | Backend Models/Serializer; Frontend `recipe.request.ts`, `recipe.types.ts`; AI-Prompts |
| D-009 | P1 | Sind `-1`, `-2`, `-3` Teil des API-Datenmodells? | Favoriten, alle Rezepte und keine Auswahl werden als Sonderwerte behandelt; `-2` wird gespeichert | Frontend `categories.service.ts:5-8,31-35`; `recipe.request.ts:82-92` |
| D-010 | P1 | Wie lautet der Bildvertrag? | Base64, MIME-Typ, Thumbor-Location, führende Slashes, S3-URL und Löschung sind nicht einheitlich definiert | Backend `image_to_recipe.py:37-88`; Frontend `recipe.request.ts:8-25` |
| D-011 | P0 | Welche Limits und erlaubten Ziele gelten für OCR, Bildupload und Website-Import? | Große synchron verarbeitete Payloads, externe Kosten, `visit_website`, SSRF und Prompt Injection sind nicht ausreichend begrenzt | Backend `views.py`, `image_to_recipe.py`, `web_to_recipe.py`, Serializer |
| D-012 | P1 | Wie lange sind Share-Secrets gültig und wie werden sie widerrufen? | Shares haben Snapshot-Semantik, aber keine erkennbare TTL, Revocation oder Löschroute | Backend `models.py:20-30`, `views.py:195-266` |
| D-013 | P1 | Sind Preise, Credits, Sharing-Entitlements und Billing aktuell oder nur geplant? | Die Landingpage bewirbt Preise und Regeln, die App zeigt Sharing ohne Tarifprüfung und enthält keine sichtbare Billing-Integration | Index `index.html:532-559`; Frontend `subscribe.vue`, `recipe.vue` |
| D-014 | P0 | Welche Rechtsquelle, Domain und Freigabeverantwortung ist verbindlich? | Legal-Inhalte werden extern geladen; `.app` und `.net` tauchen in unterschiedlichen Verweisen auf | Index `legal.html:89-125`; externe Legal-Links |
| D-015 | P1 | Ist Just Cook Web-App, PWA, native Android-App, iOS-App oder eine Kombination? | Capacitor-Konfiguration existiert, Android ist nur minimal vorhanden, iOS fehlt; Offlinefähigkeit ist nicht belegt | Frontend `capacitor.config.ts`, `android/`, `vite.config.ts`, `pwa.service.ts` |
| D-016 | P1 | Wie verhalten sich Better-Auth-Identität, Django-Admin und Löschung? | Fachliche Tabellen speichern nur String-IDs; Django `auth_user` ist getrennt; Foreign Keys und Cascade-Löschung fehlen | Backend Migrationen `0004` bis `0006`, Modelle, Admin |
| D-017 | P2 | Wer besitzt zentrale Dokumente, Reviews und fachliche Freigaben? | Ohne Product-, Engineering-, Operations-, Security- und Legal-Owner driften Verträge erneut auseinander | alle Repositories; bisher keine zentrale Ownership-Datei |

## Empfohlene Reihenfolge der Entscheidungen

1. D-001 und D-002: Umgebungen, Domains und Share-Ziel
2. D-005 und D-006: Identität, Token und Datenisolierung
3. D-004 und D-016: Datenbanken, Migrationen und Benutzerlebenszyklus
4. D-003: Runtime, Dependency- und Buildquelle
5. D-008 bis D-012: API-, Daten-, Medien- und KI-Verträge
6. D-013 und D-014: Produktpreise, Billing und Rechtsveröffentlichung
7. D-015 und D-017: Plattformumfang und Ownership

## Definition einer abgeschlossenen Entscheidung

Eine Entscheidung gilt erst als abgeschlossen, wenn:

- eine verantwortliche Rolle genannt ist;
- Entscheidung, Datum und Gültigkeitsbereich dokumentiert sind;
- betroffene Repositories und Umgebungen genannt sind;
- die Entscheidung mit Datum und Begründung im jeweils zuständigen Dokument
  festgehalten und von `docs/README.md` verlinkt ist;
- API-, Daten-, Security-, Betriebs- und Produktdokumente aktualisiert sind;
- mindestens ein Test oder manueller Nachweis die Umsetzung prüft;
- alte widersprüchliche Dokumente markiert oder archiviert wurden.
