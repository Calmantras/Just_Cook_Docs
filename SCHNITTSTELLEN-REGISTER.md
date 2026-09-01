# Schnittstellenregister: aktueller Stand und Ziel für die Referenz

| Feld | Wert |
|---|---|
| Status | `IST` mit markierten `RISIKO`- und `OFFEN`-Stellen |
| Zweck | Gemeinsame Arbeitsgrundlage für Frontend-, Backend- und Auth-Vertrag |
| Kanonische spätere Referenz | `docs/07-api/`, ergänzt durch die Bereiche `03-auth`, `04-backend` und `05-frontend` |
| Quellenstand | Backend `src/api/urls.py`, Views/Serializer; Frontend Requests/Services; Auth-Konfiguration |

Dieses Register beschreibt den gefundenen Istvertrag. Es ist noch keine
Freigabe, dass der Vertrag fachlich oder sicherheitstechnisch richtig ist.

## 1. Gemeinsamer Request-Vertrag

### Rezept- und Kategorie-API

```http
Content-Type: application/json; charset=UTF-8
Authorization: Bearer <Better-Auth-JWT>
```

Das Frontend erzeugt die Header in
`Just_Cook_Frontend/src/public/ts/requests/base.request.ts`. Das Backend
liest den Benutzer nicht aus `request.user`, sondern aus
`request.user_payload`. Der Decorator `auth()` setzt diesen Kontext in
`Just_Cook_Backend/src/api/util/user/token_manager.py`.

### Besitzmodell

Alle geschützten Rezept- und Kategorieabfragen verwenden die aus dem JWT
gelesene Benutzer-ID als Stringfilter. Es gibt keine sichtbare Rollenlogik.
`Recipe`, `Category` und `RecipeShare` besitzen keine Foreign-Key-Beziehung
zum Better-Auth-Benutzer.

### Fehlergrundlage

Aktuell sind mindestens folgende Statuscodes im Code erkennbar:

| Status | Bedeutung im aktuellen Code |
|---|---|
| `200` | erfolgreiche Abfrage, Änderung, Löschung oder Share-Erstellung |
| `201` | erfolgreiche Rezeptanlage oder OCR-zu-Rezept-Antwort |
| `400` | Serializer- oder Eingabefehler |
| `401` | fehlender, falsch formatierter, ungültiger oder abgelaufener JWT |
| `404` | Rezept, Kategorie oder Share nicht gefunden; teilweise auch falscher Status für Validierungsfehler |
| `500` | ungefangene externe Fehler oder Zugriff auf `None` möglich |

Der Sollvertrag muss einen einheitlichen Fehlerkörper und eine zentrale
Statusauswertung im Frontend festlegen. Der aktuelle Request-Layer gibt
teilweise `HttpResponse` weiter, ohne `response.status` zu prüfen.

## 2. Rezept- und KI-Endpunkte

| Methode | Pfad | Auth | Aktueller Request | Aktuelle Antwort | Quellen / Befund |
|---|---|---|---|---|---|
| `POST` | `/api/getocrtext/` | JWT | `{ "image": "<base64>" }` | `{ "ocrtext": "<String>" }`, `200` | Backend `views.py:167-180`; Bild wird zu Thumbor und Cerebras gesendet |
| `POST` | `/api/ocrtorecipe/` | JWT | `{ "ocr": "<Text>" }` | roher String, `201` | Backend `views.py:147-164`; Prompt fordert XML, Frontend parst JSON |
| `POST` | `/api/webtorecipe/` | JWT | `{ "website": "<URL>" }` | `{ "ocrtext": "<JSON-String>" }`, `200` | Backend `views.py:182-193`; Groq nutzt `visit_website` |
| `POST` | `/api/getrecipe/` | JWT | `{ "id": 123 }` | `{ id, user, title, body, image, categories }`, `200` | Backend `views.py:16-31`; Frontend erwartet aktuell `recipemd`/`category` |
| `PUT` | `/api/editrecipe/` | JWT | `{ "id", "title", "body", "image", "categories" }` | Serializerdaten, `200`; ungültig derzeit `404` | Backend `views.py:34-71`; Existenz und Fehlersemantik prüfen |
| `DELETE` | `/api/deleterecipe/` | JWT | `{ "id": 123 }` | `{ "success": true }`, `200` | Backend `views.py:113-129`; auch bei null gelöschten Datensätzen möglich |
| `GET` | `/api/getrecipes/` | JWT | kein Body | Array aus Rezeptobjekten, `200` | Backend `views.py:132-144`; Frontend mappt `body` und `categories` |
| `POST` | `/api/postrecipe/` | JWT | `{ "title", "body", "image", "categories" }` | Rezeptobjekt mit neuer `id`, `201` | Backend `views.py:74-110`; Bildupload vor DB-Speicherung |
| `POST` | `/api/sharerecipe/` | JWT | `{ "id": 123 }` | `{ "success": "<share-secret>" }`, `200` | Backend `views.py:195-224`; erzeugt dauerhaften Snapshot |
| `GET` | `/api/getsharerecipe/` | öffentlich | `?secret=<secret>` | HTML-Seite | Backend `views.py:226-243`; Route ist keine `api_view`-Funktion |
| `GET` | `/api/getsharerecipe/data/` | öffentlich | `?secret=<secret>` | `{ "title", "body", "image" }`, `200` | Backend `views.py:246-266`; Secret ist Bearer-Zugriff |

## 3. Kategorie-Endpunkte

| Methode | Pfad | Auth | Aktueller Request | Aktuelle Antwort | Quellen / Befund |
|---|---|---|---|---|---|
| `POST` | `/api/addcategory/` | JWT | `{ "category": "Vegetarisch" }` | `{}`, `200` | Backend `view_category/views.py:12-29`; Frontend versucht Responseobjekt zu pushen |
| `GET` | `/api/getcategories/` | JWT | kein Body | `[{ "id": 1, "category": "Vegetarisch" }]`, `200` | Backend `view_category/views.py:51-59` |
| `DELETE` | `/api/deletecategory/` | JWT | `{ "id": 1 }` | `{ "success": true }`, `200` | Backend `view_category/views.py:32-48` |
| `PUT` | `/api/editcategory/` | JWT | `{ "id": 1, "category": "Neu" }` | `{ "category": "Neu" }`, `200` | ID wird in Response nicht mitgeliefert |
| `PUT` | `/api/update_recipe_category/` | JWT | `{ "recipeid": 123, "categories": "1,4,-1" }` | `{ "success": true }`, `200` | Ownership der Kategorie-IDs wird nicht geprüft; fehlendes Rezept kann `500` auslösen |

## 4. Auth-Schnittstellen

Der Auth-Service mountet Better Auth unter:

```text
/api/auth/{*any}
```

Die konkrete Route-Menge wird von Better Auth `1.7.1` generiert und muss bei
Versionsänderungen erneut verifiziert werden. Für Just Cook sind mindestens
diese Routen relevant:

| Methode | Pfad | Zweck |
|---|---|---|
| `POST` | `/api/auth/sign-up/email` | E-Mail-/Passwort-Registrierung |
| `POST` | `/api/auth/sign-in/email` | E-Mail-/Passwort-Login |
| `POST` | `/api/auth/sign-in/social` | Google-/Discord-Login oder Registrierung |
| `GET`/`POST` | `/api/auth/callback/google` und `/discord` | OAuth-Callback |
| `GET` | `/api/auth/verify-email` | E-Mail-Verifikation |
| `POST` | `/api/auth/send-verification-email` | möglicher Resend-Flow; im Frontend derzeit nicht verwendet |
| `POST` | `/api/auth/request-password-reset` | Reset-Mail anfordern |
| `GET` | `/api/auth/reset-password/:token` | Reset-Token verarbeiten |
| `POST` | `/api/auth/reset-password` | neues Passwort setzen |
| `GET` | `/api/auth/get-session` | Session prüfen |
| `POST` | `/api/auth/sign-out` | Session beenden |
| `GET` | `/api/auth/token` | JWT für Backend-API beziehen |
| `GET` | `/api/auth/jwks` | öffentliche JWT-Schlüssel |
| `GET` | `/api/auth/ok` | möglicher Liveness-/Health-Endpunkt |

Die stabil zu dokumentierenden Daten sind deshalb nicht nur die Pfade,
sondern auch Better-Auth-Version, Konfiguration, Cookies, Redirects,
Statuscodes, Claims und Testfälle.

## 5. Datenformate

### 5.1 `Recipe`

Backendmodell:

```json
{
  "id": 123,
  "user": "better-auth-user-id",
  "title": "Pasta",
  "body": "{\"recipe_title\":\"Pasta\",\"ingredients\":[],\"instructions\":[]}",
  "image": "/image/hash/pasta.jpg",
  "categories": "1,4,-1"
}
```

Frontendtyp:

```ts
interface Recipe {
  id: number;
  title: string;
  body: string;
  image: string;
  categories: number[];
  ingredients?: string[];
  instructions?: string[];
}
```

Quelle: `Just_Cook_Frontend/src/public/ts/types/recipe.types.ts` und
`Just_Cook_Backend/src/api/view_recipe/models.py`.

### 5.2 `RecipeBody`

Der aktuelle Frontend-Parser verlangt:

```json
{
  "recipe_title": "Pasta",
  "ingredients": ["200 g Nudeln"],
  "instructions": ["Nudeln kochen"]
}
```

Der Backend-Serializer validiert den inneren JSON-String nicht. Der
Transformationprompt `generate_recipe.txt` fordert hingegen XML-Tags. Das
ist eine P0-Vertragsentscheidung, bevor `api-reference.md` freigegeben wird.

### 5.3 Kategorien

```text
Backend:  "1,4,7,-1"
Frontend: [1, 4, 7, -1]
```

Vorläufige Frontend-Sonderwerte:

| Wert | aktuelle Bedeutung |
|---|---|
| `-1` | Favoriten |
| `-2` | alle beziehungsweise „Meine Rezepte“ |
| `-3` | keine Auswahl |
| positive Zahl | Backend-Kategorie-ID |

Es muss entschieden werden, ob Sonderwerte Teil eines stabilen API-Vertrags
sind oder nur Frontend-Filterzustand bleiben. `-2` wird beim Posten aktuell in
den gespeicherten Kategorienstring aufgenommen.

### 5.4 Bild

Das Frontend sendet neue Bilder als Base64 ohne Data-URL-Präfix. Das Backend
lädt sie mit `Content-Type: image/jpeg` zu
`https://images.just-cook.net/image` und speichert den relativen
`Location`-Pfad. Das Frontend setzt `S3_URL` davor. Der Vertrag muss
MIME-Typ, Größe, führende Slashes, Löschung und Fehlerfälle festlegen.

## 6. Auth- und Browservertrag

| Thema | Aktueller Stand | Muss im Sollvertrag stehen |
|---|---|---|
| Session | Better-Auth-Cookie, Cross-Origin-Konfiguration | Cookie-Domain, `Secure`, `SameSite`, `HttpOnly`, Proxy und Localhost-Verhalten |
| API-Token | `/api/auth/token`, Speicherung als `just-cook.jwt` in `localStorage` | TTL, Rotation, 401, XSS-Risiko, alternative Speicherung |
| Backend-Trust | JWKS unter `<BETTER_AUTH_URL>/api/auth/jwks` | fester Issuer, Audience, Algorithmen, Claims und Key-Cache |
| Logout | Session und lokaler Token werden beendet/entfernt | Verhalten bereits ausgestellter JWTs und Cache-Löschung |
| Registrierung | E-Mail-Verifikation im Auth-Service erforderlich | ungeschützte Zielseite, Resend und Social-Login-Regel |
| CORS | Auth aus `TRUSTED_ORIGINS`, Backend mit statischen Origins | getrennte Werte pro Umgebung und keine Produktions-/Testvermischung |

## 7. Contract-Test-Matrix

Die spätere Contract-Test-Dokumentation muss mindestens folgende Fälle
referenzieren:

| Vertrag | Positivfall | Negativfall |
|---|---|---|
| JWT/JWKS | gültiges Token und Schlüsselrotation | falsche Signatur, falscher Issuer/Audience, abgelaufen, falscher Algorithmus |
| Rezeptliste | Nutzer sieht eigene Rezepte | fremde Benutzer-ID und leerer Bestand |
| Einzelrezept | `body` und `categories` werden korrekt gemappt | nicht vorhandenes/fremdes Rezept |
| Rezeptanlage | JSON-Body, Bild `none`, Kategorien | zu große/ungültige Payload, Bild- oder DB-Fehler |
| Kategorie | Create liefert vereinbartes Objekt | leere Kategorie, fremde ID, leere Kategorienliste |
| OCR | valides JSON-Schema | XML, kaputte JSON-Antwort, KI-Timeout |
| URL-Import | erlaubte HTTPS-URL | interne/private URL, Redirect, Timeout, Prompt Injection |
| Sharing | Secret erzeugt und Daten abgerufen | falsches, abgelaufenes oder widerrufenes Secret |
| Browsercache | Benutzer A/B sauber getrennt | Logout und API-Fehler mit altem Cache |

## 8. Verbindliche Entscheidungen vor API-Freigabe

- API-Version und Deprecation-Policy
- kanonische Response-Feldnamen
- kanonisches Recipe-Body-Format
- Fehlerobjekt und Statussemantik
- Maximalgrößen und Timeouts
- Kategorie-Sonderwerte
- Bild-URL- und Pfadnormalisierung
- Share-TTL und Widerruf
- JWT-Claims und Revocation

## 9. Öffentliche Schnittstellen der Landingpage

`Just_Cook_Index` bietet keine JSON-API und keine Authentifizierung. Seine
Schnittstellen sind statische HTTP-Routen, Assets und externe Links.

| Pfad oder Ziel | Verhalten | Status / Randbedingung |
|---|---|---|
| `/`, `/index`, `/index.html` | statische Landingpage | bevorzugte Canonical-URL ist `https://just-cook.app/` |
| `/legal`, `/legal.html` | statische Legal-Hülle mit vier extern geladenen Texten | `/legal.html` wird aktuell nicht sichtbar auf `/legal` vereinheitlicht |
| `/impressum` | Redirect auf `/legal#impressum` | HTTP `302` |
| `/datenschutz` | Redirect auf `/legal#datenschutz` | HTTP `302` |
| `/legal#agb`, `/legal#widerruf` | Browserfragment auf derselben Legal-Seite | Fragment wird nicht an den Server gesendet |
| unbekannter Pfad | interne Fehlerseite | `/404.html` ist intern und direkt nicht abrufbar |
| `/fav/*`, `/images/*` | lokale Marken-, Manifest- und Marketingassets | lange `Cache-Control`-Policy für ausgewählte Erweiterungen |
| `https://app.just-cook.net/login` | CTA zur eigentlichen App-Anmeldung | kein Token- oder SSO-Übergabevertrag aus der Landingpage belegt |
| `https://app.just-cook.net/register` | CTA zur eigentlichen App-Registrierung | kein Token- oder SSO-Übergabevertrag aus der Landingpage belegt |
| `https://www.it-recht-kanzlei.de/js/itrk-legaltext.js` | externer Legal-Loader | kein SRI/CSP im Repository sichtbar |
| `https://unpkg.com/ionicons/...` | Icons der 404-Seite | externe Browserabhängigkeit |

Die spätere Landingpage-Dokumentation muss zusätzlich DNS, TLS, Reverse Proxy,
CDN, Cacheinvalidierung, externe CORS-Annahmen und Ausfallverhalten nennen.
