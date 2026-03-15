# Datenschutz-Informationsbericht — Macobu
## Für die Datenschutzbeauftragte zur Prüfung

**Erstellt von:** Alfred Schenkel und Jan-Hendrik Schoendorff GbR
**Datum:** März 2026
**Zweck:** Vollständige Übersicht über alle Datenverarbeitungen der App Macobu zur datenschutzrechtlichen Prüfung

---

## 1. Über die App

**Macobu** ist eine kostenfreie mobile App (iOS + Android), die Nutzerinnen und Nutzern ermöglicht:
- Eine persönliche Bibliothek mit Büchern, Manga und Comics zu verwalten
- Neue Bücher über einen personalisierten Entdecken-Feed zu entdecken
- Bücher auf der Wunschliste zu suchen und zu kaufen (lokale Buchhandlungen, Online-Shops)
- Bücher mit anderen Nutzern direkt zu tauschen (1:1-Tausch, Direkt-Tausch)
- Bücher über ein Credit-System zu tauschen (Knopftausch)
- Mit Tauschpartnern und Followern privat zu kommunizieren (Chat)
- Bücher zu bewerten und ein Leseziel zu setzen
- Andere Nutzer zu folgen und Abzeichen (Badges) zu sammeln

**Technische Plattform:** Supabase (PostgreSQL-Datenbank, Authentifizierung, serverseitige Funktionen) auf EU-Servern in Frankfurt am Main (Region eu-central-1).

**Verantwortliche (gemeinsam Verantwortliche nach Art. 26 DSGVO):**
Alfred Schenkel und Jan-Hendrik Schoendorff GbR
support@macobu.de

---

## 2. Überblick: Was wird gespeichert — Was wird NICHT gespeichert

### ✅ Was gespeichert wird

| Datenkategorie | Speicherort | Sichtbarkeit | Löschung |
|---|---|---|---|
| E-Mail-Adresse | Supabase Auth | Nur intern / nicht öffentlich | Bei Account-Löschung |
| Passwort-Hash (bcrypt) | Supabase Auth | Nur intern | Bei Account-Löschung |
| Einwilligungs-Zeitstempel (ToS, DSGVO) | profiles-Tabelle | Nur intern | Bei Account-Löschung |
| Benutzername | profiles-Tabelle | Öffentlich für angemeldete Nutzer | Bei Account-Löschung |
| Anzeigename (optional) | profiles-Tabelle | Öffentlich | Bei Account-Löschung |
| Profilbild/Avatar (optional) | profiles-Tabelle (Asset-Pfad) | Öffentlich | Bei Account-Löschung |
| Biografie (optional, max. 500 Zeichen) | profiles-Tabelle | Öffentlich | Bei Account-Löschung |
| Lesepräferenzen (Genres, Epochen, Medientyp) | profiles-Tabelle | Nur für Personalisierung | Bei Account-Löschung |
| Leseziel (optional) | profiles-Tabelle | Optional öffentlich | Bei Account-Löschung |
| Jährliche Lesezahl | profiles-Tabelle | Öffentlich auf Profil | Rücksetzung jährlich; Löschung bei Account-Löschung |
| Statistiken (gelesene Bücher, abgeschlossene Tausche) | profiles-Tabelle | Öffentlich | Bei Account-Löschung |
| Bewertungszähler (ratings_given) | profiles-Tabelle | Öffentlich (Zahl) | Bei Account-Löschung |
| Angepinnter Badge | profiles-Tabelle | Öffentlich | Bei Account-Löschung |
| Knopf-Guthaben (credits, credits_pending) | profiles-Tabelle | Nur für Nutzer selbst | Bei Account-Löschung |
| Bibliothek (Bücher, Status, Notizen, Bewertungen) | user_books-Tabelle | Nur für Nutzer selbst | Bei Account-Löschung |
| Archiv / Leseverlauf | archive-Tabelle | Optional öffentlich (letzte 3) | Bei Account-Löschung |
| Wunschliste | wishlist-Tabelle | Nur für Nutzer selbst | Bei Account-Löschung |
| Tauschsuche (Sprache, Suchstatus) | wishlist_trade_seeks-Tabelle | Nur intern für Matching | Bei Deaktivierung oder Account-Löschung |
| Swipe-Verhalten (Entdecken-Feed) | discover_seen-Tabelle | Nur intern für Personalisierung | Bei Account-Löschung |
| Tauschanfragen (Status, Zeitstempel, Buchinfos) | trade_requests-Tabelle | Nur für Tauschpartner | Bei Account-Löschung |
| Lieferadresse (Name, Straße, PLZ, Stadt, Land) | trade_addresses-Tabelle | Nur für Tauschpartner | **Sofort nach Tausch-Abschluss automatisch gelöscht** |
| Sendungsnummer | trade_requests-Tabelle | Nur für Tauschpartner | 90 Tage nach Abschluss |
| Credit-Buchungsprotokoll (Knopftausch) | credit_transactions-Tabelle | Nur für Nutzer selbst + intern | Bei Account-Löschung |
| Tier-Bonus-Status | profiles-Tabelle | Nur intern | Bei Account-Löschung |
| Follow-Beziehungen | user_follows-Tabelle | Counts öffentlich | Bei Account-Löschung |
| Chat-Nachrichten (Inhalt, Zeitstempel) | chat_messages-Tabelle | Nur Gesprächspartner | 30 Tage nach Account-Löschung; gemeldete Inhalte 30 Tage nach Meldung |
| Chat-Metadaten (Lesebestätigung, Löschstatus) | chat_messages-Tabelle | Nur Gesprächspartner | 90 Tage nach Account-Löschung |
| Konversationen (Übersicht, Partner-ID, Zeitstempel) | direct_conversations-Tabelle | Nur beteiligte Nutzer | Bei Account-Löschung |
| Nachrichten-Meldungen | message_reports-Tabelle | Nur intern (Admin) | Mindestens 30 Tage nach Meldung; bei Account-Löschung des meldenden Nutzers weiterhin aufbewahrt |
| Filterprotokoll bei Filterverstoß (kein Inhalt, nur Zeitstempel + Regelname + Nutzer-ID) | Intern | Nur intern | 90 Tage |
| Sanktionen (Verwarnungen, Sperren) | Intern (sanctions-Tabelle) | Nur intern (Admin) | Bei Account-Löschung |
| FCM-Gerätetoken (Push-Benachrichtigungen) | push_tokens-Tabelle | Nur intern | Bei Abmeldung oder Account-Löschung |
| Admin-Broadcasts (Systembenachrichtigungen) | macobu_broadcasts-Tabelle | Alle angemeldeten Nutzer | Administrativ steuerbar |
| Buchmetadaten (ISBN, Titel, Autoren, Cover) | books-Tabelle (zentral, nicht nutzerspezifisch) | Allgemein zugänglich | Nicht personenbezogen; kein Löschanspruch |

---

### ❌ Was NICHT gespeichert wird

| Was NICHT gespeichert wird | Erklärung |
|---|---|
| GPS-/Standortdaten | Werden ausschließlich transient im Arbeitsspeicher des Geräts verarbeitet, um die Google Places API anzufragen; werden sofort verworfen und nie in der Datenbank gespeichert |
| Klartextpasswörter | Passwörter werden ausschließlich als bcrypt-Hash gespeichert — der Anbieter hat keinen Zugriff auf das ursprüngliche Passwort |
| Nachrichteninhalte bei Filterverstoß | Wenn eine Nachricht gegen die Filterregeln verstößt (z.B. enthält einen Link), wird die Nachricht vollständig verworfen — nur ein anonymisiertes Protokoll (Regelname, Zeitstempel, Nutzer-ID, **kein Inhalt**) wird gespeichert |
| Lieferadressen nach Tausch-Abschluss | Adressen werden unmittelbar nach Tausch-Abschluss automatisch und dauerhaft gelöscht — danach hat weder der Anbieter noch der Tauschpartner Zugriff |
| Zahlungsdaten | Die App ist kostenlos; es gibt keine Zahlungsfunktionen; keine Bankdaten oder Kreditkartendaten werden erfasst |
| Echte Klarnamen (außer bei freiwilliger Angabe) | Benutzername und Anzeigename werden vom Nutzer selbst gewählt; der Anbieter verlangt keine Identitätsprüfung |
| Gerätedaten über FCM hinaus | Außer dem FCM-Token werden keine Geräteinformationen (IMEI, MAC-Adresse, Betriebssystemversion, etc.) gespeichert |
| Kommunikationsdaten außerhalb des Chats | E-Mail-Kommunikation zwischen Nutzern findet nicht in der App statt; Telefonnummern sind als Nachrichteninhalt verboten und werden gefiltert |
| Profilbilder als Upload | Nutzer wählen ein Avatar aus einer vordefinierten Bibliothek innerhalb der App; es werden keine Fotos hochgeladen |
| Surf- oder Browserdaten | Die App ist kein Browser-Dienst; kein Tracking von externen Webseitenbesuchen |
| Werbedaten oder Tracking-IDs | Es gibt keine Werbung, kein Ad-Tracking, keine Analytics-Tools von Drittanbietern (wie Google Analytics, Meta Pixel etc.) |
| Daten minderjähriger Nutzer | Das Mindestalter beträgt 16 Jahre; bei begründetem Verdacht auf Minderjährigkeit wird der Account gesperrt |

---

## 3. Alle Features und Datenzuflüsse — ausführlich erklärt

### 3.1 Registrierung und Authentifizierung

**Was passiert:** Nutzer registrieren sich mit E-Mail-Adresse und Passwort. Zur Aktivierung wird eine Bestätigungs-E-Mail versendet. Der Zeitstempel der Zustimmung zur Datenschutzerklärung und den AGB wird gespeichert.

**Rechtsgrundlage:** Art. 6 Abs. 1 lit. b DSGVO (Vertragserfüllung), Art. 7 Abs. 1 DSGVO (Einwilligungsnachweis)

**Daten:** E-Mail, Passwort-Hash, Einwilligungs-Zeitstempel, `accepted_tos_version`, `is_onboarded`-Flag

**Technischer Dienstleister:** Supabase Auth (EU-Server Frankfurt)

**Besonderheit:** Der Anbieter sieht das Klartextpasswort zu keiner Zeit. Passwort-Resets erfolgen über einen E-Mail-Link. Der JWT-Token (Sitzungstoken) läuft ab und wird nicht dauerhaft gespeichert.

---

### 3.2 Nutzerprofil

**Was passiert:** Nutzer können ein öffentliches Profil aufbauen. Pflichtfeld ist nur der Benutzername. Alles weitere (Anzeigename, Avatar, Bio) ist optional.

**Daten:** `username` (Pflicht), `display_name` (optional), `avatar_url` (Asset-Pfad aus vordefinierten Avataren, optional), `bio` (optional, max. 500 Zeichen), `preferred_genres[]`, `preferred_eras[]`, `interests[]` (Lesepräferenzen, aus Onboarding)

**Öffentliche Daten (für andere angemeldete Nutzer sichtbar):** Benutzername, Anzeigename, Avatar, Bio, Statistiken (gelesene Bücher, abgeschlossene Tausche), Follower-/Following-Zahl, Leseziel (steuerbar), letzte 3 gelesene Bücher (steuerbar), angepinnter Badge, Tier-Level

**Nicht-öffentliche Daten:** E-Mail, Passwort-Hash, Lesepräferenzen, Bibliotheksinhalte, Wunschliste, Swipe-Verlauf, Chat-Nachrichten, Lieferadressen, Credits

---

### 3.3 Bibliothek und Leseaktivität

**Was passiert:** Nutzer fügen Bücher, Manga und Comics ihrer Bibliothek hinzu. Bücher können mit Status versehen werden (ungelesen, gelesen, zum Tausch, etc.). Nutzer können Bücher bewerten (6 Kategorien: Schärfe/Spannung/Action/Drama/Dunkelheit/Humor, jeweils 0–10), persönliche Notizen hinzufügen und Bücher in eigene Ordner/Sektionen organisieren.

**Besonderheit Bewertungen:** Individuelle Bewertungen sind ausschließlich für den Nutzer selbst sichtbar. Anderen Nutzern wird nur der anonymisierte Durchschnitt aller Bewertungen angezeigt. Es findet keine Profilierung auf Basis der Bewertungen statt.

**Besonderheit Archiv:** Wenn ein Buch als „gelesen" markiert wird, wird es automatisch ins Archiv eingetragen (DB-Trigger). Das Archiv bleibt erhalten, auch wenn das Buch später aus der Bibliothek entfernt oder getauscht wird.

**Daten:** `user_books`-Tabelle (user_id, book_id, status, media_type, condition, notes, added_at, rating_spice/action/drama/dark/suspense/humor, ranking, trade_language, read_year, folder_id, section_id); `archive`-Tabelle (user_id, book_id, media_type, source, added_at); `library_folders`- und `library_sections`-Tabellen

---

### 3.4 Entdecken-Feed und Wunschliste

**Was passiert:** Nutzer sehen Buchvorschläge basierend auf ihren Lesepräferenzen aus dem Onboarding. Sie können Bücher mit „Merken" (Wunschliste) oder „Überspringen" markieren. Die Entscheidung wird gespeichert, damit dasselbe Buch nicht erneut vorgeschlagen wird.

**Personalisierung:** Die Personalisierung basiert ausschließlich auf den vom Nutzer selbst im Onboarding angegebenen Präferenzen (Genres, Epochen, Medientyp, Sprache). Es findet kein Behavioral Tracking oder Profiling durch Drittanbieter statt.

**Wunschliste → Kaufmöglichkeiten:** Nutzer können für Wunschlisten-Bücher auf Amazon, Thalia oder Panini verlinken. Diese Klicks öffnen den externen Browser — es werden keine Affiliate-Codes oder Tracking-Parameter verwendet.

**Wunschliste → Lokale Buchhandlungen:** Nur bei aktivierter GPS-Berechtigung. Koordinaten werden transient verarbeitet (niemals gespeichert).

**Daten:** `wishlist`-Tabelle, `discover_seen`-Tabelle (pool_id, seen_at)

---

### 3.5 Tauschbörse (1:1 Direkt-Tausch)

**Was passiert:** Nutzer können Bücher aus ihrer Bibliothek zum Tausch anbieten. Das System sucht automatisch nach passenden Tauschpartnern (Matching-Algorithmus: Nutzer A möchte Buch X, das Nutzer B besitzt; Nutzer B möchte Buch Y, das Nutzer A besitzt).

**Trade-Flow:**
1. **pending** — Tauschanfrage wird gesendet
2. **confirmed** — Beide Nutzer bestätigen den Tausch
3. **address_shared** — Beide Nutzer teilen ihre Lieferadresse
4. **shipped** — Beide Nutzer bestätigen den Versand und geben die Sendungsnummer ein
5. **completed** — Beide Nutzer bestätigen den Erhalt

**Lieferadresse — Besonderes Datenschutzrisiko:**
Die Lieferadresse ist ein besonders schutzwürdiges personenbezogenes Datum (Wohnanschrift oder Packstation). Sie wird:
- Ausschließlich dem Tauschpartner (nicht Dritten) angezeigt
- Nur für den Zeitraum zwischen Adresseingabe und Tausch-Abschluss gespeichert
- Unmittelbar nach Tausch-Abschluss automatisch und dauerhaft gelöscht (DB-Trigger)
- Mit einer Bestätigungsmaske vor dem Absenden angezeigt, damit Nutzer die Adresse noch einmal prüfen können

**HERE Geocoding:** Für die Adresseingabe wird ein Autocomplete-Service verwendet. Die eingetippten Adresstexte werden über einen serverseitigen Proxy an HERE übermittelt; kein Nutzerkonto bei HERE wird erstellt; keine Nutzer-ID wird übermittelt.

**Daten:** `trade_requests`-Tabelle, `trade_addresses`-Tabelle (nach Abschluss gelöscht), `wishlist_trade_seeks`-Tabelle

---

### 3.6 Knopftausch (Credit-basierter Tausch)

**Was passiert:** Nutzer können über ein internes Credit-System Bücher von anderen Nutzern beziehen, ohne selbst ein Buch anzubieten. Credits werden als „Knöpfe" bezeichnet.

**Knöpfe erhalten:**
- 1 Knopf automatisch 12 Stunden nach Registrierung (initialer Bonus)
- 1 Knopf bei Erstfreischaltung der Tiers „Story Hunter", „Connoisseur", „Macobu Legend"
- 1 Knopf als anbietender Nutzer (User B) nach erfolgreichem Abschluss eines Knopftauschs

**Knöpfe ausgeben:**
- 1 Knopf pro Knopftausch-Anfrage (wird eingefroren bis Abschluss)
- Bei Abbruch/Ablehnung: Automatische Rückerstattung

**Wichtig:** Knöpfe haben keinen monetären Wert; es findet kein Zahlungsvorgang statt.

**Buchungsprotokoll:** Jede Transaktion wird transparent protokolliert (credit_transactions-Tabelle). Der Nutzer kann sein Buchungsprotokoll in der App einsehen.

**Daten:** `profiles.credits`, `profiles.credits_pending`, `profiles.highest_tier_credited`; `credit_transactions`-Tabelle (user_id, amount, reason, trade_id, created_at)

---

### 3.7 Chat (Nachrichten)

**Was passiert:** Nutzer können privat miteinander kommunizieren. Der Chat ist in zwei Varianten verfügbar:
- **Tausch-Chat:** Automatisch verfügbar, sobald eine gemeinsame Tauschanfrage besteht
- **Direkt-Nachrichten:** Verfügbar zwischen Nutzern, die sich gegenseitig folgen

**Schutzmaßnahmen:**
- Chat wird erst 12 Stunden nach Kontoerstellung freigeschaltet (Missbrauchsschutz)
- Maximal 2.000 Zeichen pro Nachricht
- Maximal 60 Nachrichten pro Stunde (Rate-Limiting)
- Keine externen Links, Telefonnummern, E-Mail-Adressen oder Messenger-Einladungen erlaubt (automatische Filterung vor der Speicherung)
- Keine Ende-zu-Ende-Verschlüsselung (bewusste Entscheidung, um automatische Inhaltsmoderation und manuelle Prüfung gemeldeter Nachrichten zu ermöglichen — in der Datenschutzerklärung transparent gemacht)
- Nachrichten werden verschlüsselt übertragen (TLS) und verschlüsselt gespeichert (AES-256)

**Moderation:**
- Nutzer können Nachrichten melden (Long-Press)
- Gemeldete Nachrichten werden von Mitarbeitern manuell geprüft
- DSA Art. 16: Meldungs-ID wird dem meldenden Nutzer mitgeteilt

**Soft-Delete:** Nutzer können Gespräche für sich selbst ausblenden (Swipe-to-Delete in der Chat-Übersicht). Die Nachricht bleibt technisch für eventuelle Streitfälle erhalten.

**Daten:** `direct_conversations`-Tabelle, `chat_messages`-Tabelle; `message_reports`-Tabelle bei Meldungen

---

### 3.8 Soziale Funktionen (Follow-System)

**Was passiert:** Nutzer können anderen Nutzern folgen. Follower- und Following-Zahlen sind öffentlich sichtbar. Das gegenseitige Folgen ist Voraussetzung für Direkt-Nachrichten.

**Daten:** `user_follows`-Tabelle (follower_id, following_id, created_at)

---

### 3.9 Achievements und Tier-System

**Tier-System:** Basiert auf der Anzahl gelesener Bücher und abgeschlossener Tausche. Berechnung erfolgt lokal in der App — kein externes Profiling.

| Tier | Bedingung |
|---|---|
| Newcomer | Standard (kein Schwellenwert) |
| Bookworm | 10 gelesene + 1 Tausch |
| Story Hunter | 25 gelesene + 5 Tausche |
| Connoisseur | 50 gelesene + 15 Tausche |
| Macobu Legend | 100 gelesene + 30 Tausche |

**Badge-System:** Basiert auf der Anzahl abgegebener Buchbewertungen (ratings_given). 5 Badges von „First Opinion" (1 Bewertung) bis „The Oracle" (100 Bewertungen). Nutzer können ein Badge anpinnen, das auf dem öffentlichen Profil sichtbar ist.

**Daten:** `profiles.read_books_count`, `profiles.completed_trades`, `profiles.ratings_given`, `profiles.pinned_badge`, `profiles.pinned_tier` — alles aus bereits verarbeiteten App-Aktivitäten abgeleitet, kein separates Profiling.

---

### 3.10 Moderation und Admin-Dashboard

**Was passiert:** Admins (interne Mitarbeiter) können über ein eingeschränktes Admin-Dashboard:
- Gemeldete Nachrichten einsehen und bearbeiten
- User-Sanktionen verhängen (Verwarnung, temporäre/permanente Sperre)
- Benutzernamen als unzulässig markieren (Nutzer wird beim nächsten Login zur Änderung aufgefordert)
- Inhalte (ISBN-basiert) in die Content Blocklist aufnehmen
- System-Broadcasts (Ankündigungen an alle Nutzer) mit Zwei-Admin-Freigabe erstellen

**Zugriffsbeschränkung:** Admin-Zugriff ist auf eingetragene Admin-User-IDs in der `app_admins`-Tabelle beschränkt. Row-Level Security (RLS) auf Datenbankebene verhindert unbefugten Zugriff.

**Daten:** `report_items`-Tabelle, `message_reports`-Tabelle, `app_admins`-Tabelle, `macobu_broadcasts`-Tabelle, Sanktions-Datensätze

---

### 3.11 Push-Benachrichtigungen

**Was passiert:** Mit Einwilligung des Nutzers werden Push-Benachrichtigungen über tauschrelevante Ereignisse gesendet (z.B. neue Tauschanfrage, Tauschanfrage bestätigt, Paket versendet, Paket erhalten).

**Kein Werbe-Push:** Es werden ausschließlich tauschrelevante Benachrichtigungen versendet. Keine werblichen, marketing-orientierten oder Engagement-Benachrichtigungen.

**Daten:** `push_tokens`-Tabelle (user_id, token, device_type, created_at)

**Drittanbieter:** Firebase Cloud Messaging (Google LLC, USA; SCCs)

---

### 3.12 Datenexport (Art. 15 + 20 DSGVO)

**Was passiert:** Nutzer können über Profil → Einstellungen → „Meine Daten exportieren" einen vollständigen JSON-Export ihrer Daten erstellen. Der Export umfasst alle in der Datenschutzerklärung genannten personenbezogenen Datenkategorien.

**Daten im Export:** Profildaten, Bibliothek (inkl. Bewertungen), Wunschliste, Archiv (Leseverlauf), Swipe-Verlauf, Chat-Nachrichten (gesendete Nachrichten), Follow-Beziehungen, Tausch-Verlauf.

---

### 3.13 Account-Löschung (Art. 17 DSGVO)

**Was passiert:** Nutzer können über Profil → Einstellungen → „Konto löschen" ihren Account und alle damit verbundenen Daten vollständig und unwiderruflich löschen. Die Löschung wird durch eine serverseitige Funktion (`delete_account()`) ausgeführt, die alle Datenbankeinträge des Nutzers entfernt.

**Ausnahme (DSA-Compliance):** Chat-Nachrichten in gemeldeten Konversationen werden für mindestens 30 Tage nach der Meldung aufbewahrt — auch wenn der Nutzer seinen Account löscht.

---

## 4. Drittanbieter-Übersicht

| Anbieter | Zweck | Übermittelte Daten | Serverstandort | Rechtsgrundlage Drittlandübermittlung |
|---|---|---|---|---|
| **Supabase, Inc.** | Datenbank, Authentifizierung, serverseitige Funktionen (Edge Functions) | Alle personenbezogenen Daten (Auftragsverarbeiter) | EU (Frankfurt am Main, Deutschland) | DPA gemäß Art. 28 DSGVO + SCCs |
| **Google LLC** (Google Books API) | Buchmetadaten-Suche | Nur ISBN / Suchtitel (kein Nutzerbezug) | USA | SCCs |
| **Internet Archive** (Open Library) | Buchmetadaten | Nur Suchtitel/ISBN | USA | Keine personenbezogenen Daten übermittelt |
| **MangaDex** | Manga-Metadaten | Nur Suchtitel/ID | Unbekannt | Keine personenbezogenen Daten übermittelt |
| **Fandom, Inc.** (ComicVine) | Comic-Metadaten | Nur Suchtitel/ID | USA | SCCs |
| **ISBNdb** | Buch-ISBN-Metadaten | Nur ISBN-Nummern | USA | Keine personenbezogenen Daten übermittelt |
| **HERE Global B.V.** | Adressvervollständigung (Trade-Adresseingabe) | Eingetippte Adresstexte (kein Nutzerkonto) | EU (Niederlande) | EU-Anbieter, kein Drittland |
| **Google LLC** (Google Places API) | Lokale Buchhandlungssuche | GPS-Koordinaten (transient, niemals gespeichert) | USA | SCCs + Einwilligung |
| **Google LLC** (Firebase Cloud Messaging) | Push-Benachrichtigungen | FCM-Token (Gerätetoken) | USA | SCCs + Einwilligung |

---

## 5. Technische und organisatorische Maßnahmen (TOMs)

| Maßnahme | Umsetzung |
|---|---|
| Verschlüsselung in der Übertragung | HTTPS/TLS für alle API-Verbindungen |
| Verschlüsselung im Speicher | AES-256 (Supabase-Standard) |
| Datenbankzugriff | Row-Level Security (RLS): Jeder Nutzer kann nur auf seine eigenen Daten zugreifen |
| Passwort-Sicherheit | bcrypt-Hashing; keine Klartextspeicherung; kein Admin-Zugriff auf Passwörter |
| API-Schlüssel | Ausschließlich serverseitig gespeichert (Supabase Secrets); niemals an die App übertragen |
| Admin-Zugriff | Beschränkt auf eingetragene Admin-IDs; separate Zugriffskontrolle via Datenbanktabelle |
| Adresslöschung | Automatischer DB-Trigger löscht Lieferadressen unmittelbar nach Tausch-Abschluss |
| Content Moderation | Automatische Echtzeit-Filterung vor der Nachrichtenspeicherung |
| Missbrauchsschutz | 12h-Wartezeit für Chat/Tausch; Rate-Limiting (60 Nachrichten/h); Sanktions-System |
| Minderjährigenschutz | Mindestalter 16 Jahre; AGB-Checkbox mit expliziter Bestätigung |

---

## 6. Speicherdauer-Übersicht

| Datenkategorie | Speicherdauer |
|---|---|
| Profil- und Kontodaten | Bis zur Account-Löschung |
| Bibliothek, Archiv, Wunschliste | Bis zur Account-Löschung |
| Swipe-Verlauf | Bis zur Account-Löschung |
| Tausch-Verlauf (ohne Adresse) | Bis zur Account-Löschung |
| **Lieferadressen** | **Sofort nach Tausch-Abschluss automatisch und dauerhaft gelöscht** |
| Sendungsnummern | 90 Tage nach Tausch-Abschluss |
| Chat-Nachrichten (Inhalt) | Bis Account-Löschung; 30 Tage nach Account-Löschung (Nachlaufzeit) |
| Chat-Metadaten | 90 Tage nach Account-Löschung |
| Gemeldete Chat-Nachrichten | Mindestens 30 Tage nach Meldung (auch bei Account-Löschung) |
| Filterprotokoll bei Verstößen | 90 Tage |
| FCM-Gerätetoken | Bis Abmeldung oder Account-Löschung |
| Einwilligungs-Zeitstempel | Bis Account-Löschung |
| Credit-Buchungsprotokoll | Bis Account-Löschung |
| Moderations-/Sanktionsdaten | Bis zur Löschung des betroffenen Accounts |

---

## 7. Betroffenenrechte — Umsetzung in der App

| Recht | Wie umgesetzt |
|---|---|
| **Art. 15 DSGVO** — Auskunftsrecht | Datenexport (JSON) direkt in der App; Profil → Einstellungen → „Meine Daten exportieren" |
| **Art. 16 DSGVO** — Berichtigung | Profildaten jederzeit editierbar unter Profil → Profil bearbeiten |
| **Art. 17 DSGVO** — Löschung | Account-Löschung jederzeit möglich; Profil → Einstellungen → „Konto löschen"; alle Daten werden serverseitig vollständig entfernt |
| **Art. 18 DSGVO** — Einschränkung | Per E-Mail-Anfrage (support@macobu.de) |
| **Art. 20 DSGVO** — Datenübertragbarkeit | JSON-Export (s.o. Art. 15) |
| **Art. 21 DSGVO** — Widerspruch | Nicht anwendbar (keine Direktwerbung, keine automatisierte Entscheidungsfindung mit Rechtswirkung) |
| **Art. 7 Abs. 3 DSGVO** — Widerruf | Push-Benachrichtigungen: jederzeit in Geräteeinstellungen widerrufbar; optionale Profilfelder: jederzeit löschbar |
| **Art. 22 DSGVO** — Automatisierte Entscheidungen | Keine automatisierten Entscheidungen mit Rechtswirkung; Tier/Badge-Berechnung ist rein informativ |
| **Art. 77 DSGVO** — Beschwerde | LDI NRW, Kavalleriestraße 2–4, 40213 Düsseldorf, https://www.ldi.nrw.de |

---

## 8. Risikobewertung / Datenschutz-Folgenabschätzung (kurze Einschätzung)

| Verarbeitung | Risikostufe | Begründung und Maßnahmen |
|---|---|---|
| Registrierung + Authentifizierung | **Niedrig** | Standard-Auth via Supabase; bcrypt-Passwörter; E-Mail-Bestätigung |
| Bibliotheksdaten | **Niedrig** | Nur für Nutzer selbst sichtbar; RLS-Schutz |
| Lieferadressen | **Mittel** | Wohnanschriften sind sensibel; Maßnahme: sofortige Löschung nach Abschluss + Bestätigungsmaske + Support-Zugriff nur bei aktiven Tauschvorgängen |
| Chat-Inhalte | **Mittel** | Inhalte sind sensibel; keine E2E-Verschlüsselung; Maßnahme: AES-256-Speicherung, TLS, automatische Inhaltsfilterung, manuelle Moderation, DSA-Compliance |
| GPS-Standortdaten | **Niedrig** | Niemals gespeichert; nur transient verarbeitet; explizite Einwilligung erforderlich |
| Credit-Buchungsprotokoll | **Niedrig** | Keine Zahlungsdaten; interne Credits ohne Geldwert; nur für Nutzer + Anbieter einsehbar |
| Moderationsdaten | **Niedrig–Mittel** | Sanktionsdaten sind sensibel für Betroffene; Maßnahme: Admin-Zugriff streng begrenzt, keine öffentliche Sichtbarkeit |
| FCM-Gerätetoken | **Niedrig** | Kein Inhaltsbezug; nur für Zustellung; Einwilligung erforderlich |

---

## 9. Kontakt

**Für Datenschutzanfragen:**
Alfred Schenkel und Jan-Hendrik Schoendorff GbR
E-Mail: support@macobu.de

**Zuständige Aufsichtsbehörde:**
Landesbeauftragte für Datenschutz und Informationsfreiheit Nordrhein-Westfalen (LDI NRW)
Kavalleriestraße 2–4, 40213 Düsseldorf
https://www.ldi.nrw.de

---

*Dieser Bericht fasst alle Datenverarbeitungen der App Macobu zusammen und richtet sich an Datenschutzfachleute zur Überprüfung. Er ist kein öffentlich zugängliches Dokument. Stand: März 2026.*
