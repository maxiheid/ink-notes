# Ink Notes

Handschriftliche Notizen im Stil von GoodNotes/OneNote – als einzelne HTML-Seite, gehostet über GitHub Pages, in Notion einbettbar und über Firebase zwischen allen Geräten synchronisiert.

**Funktionen:** Stift mit Druckempfindlichkeit (Apple Pencil / Stylus), Textmarker, Radierer (auch über die Stylus-Radiertaste), Farben, drei Strichstärken, liniertes / kariertes / gepunktetes / leeres Papier, mehrere A4-Seiten, Rückgängig/Wiederholen, Zoom, Touch wie bei GoodNotes (ein Finger scrollt, zwei Finger zoomen, nur der Stift schreibt, Handballen wird ignoriert), Export als PNG oder PDF, Backup als JSON.

## 1. Auf GitHub Pages veröffentlichen

1. Auf github.com ein neues **öffentliches** Repository anlegen, z. B. `ink-notes`.
2. `index.html` und `.nojekyll` hochladen (*Add file → Upload files*), committen.
3. *Settings → Pages → Build and deployment*: Source **Deploy from a branch**, Branch **main**, Ordner **/ (root)** → *Save*.
4. Nach ca. einer Minute ist die Seite erreichbar unter
   `https://maxiheid.github.io/ink-notes/`

Oder per Terminal:

```bash
git init && git add index.html .nojekyll README.md && git commit -m "Ink Notes"
git branch -M main
git remote add origin https://github.com/maxiheid/ink-notes.git
git push -u origin main
```

## 2. In Notion einbetten

1. In Notion `/embed` tippen, den Link einfügen:
   `https://maxiheid.github.io/ink-notes/?id=mathe`
2. Den Embed-Block unten am Rand auf die gewünschte Höhe ziehen.

**Jede `?id=` ist ein eigenes Notizbuch.** Für jede Notion-Seite eine andere id verwenden (`?id=mathe`, `?id=meeting-2026-10-26`, …). Im Menü (⋯) → *Neues Notizbuch / Embed-Link* erzeugt einen zufälligen Link zum Kopieren.

Optional: `&theme=dark` oder `&theme=light` erzwingt das Farbschema der Werkzeugleiste.

## 3. Synchronisation einrichten (Firebase, kostenlos)

Damit Webseite, Notion-Embeds und alle Geräte dieselben Notizen zeigen, werden die Striche in Firebase Firestore gespeichert, und zwar in Echtzeit: Was du auf dem iPad schreibst, erscheint sofort auch am Laptop. Ohne Firebase speichert die Seite nur lokal im jeweiligen Browser, und dann ist sogar ein Notion-Embed vom selben Notizbuch auf der Webseite getrennt.

1. <https://console.firebase.google.com> öffnen → **Projekt hinzufügen** → Name z. B. `ink-notes` (Google Analytics kann aus).
2. Links **Build → Firestore Database → Datenbank erstellen** → Standort `eur3 (europe-west)` → **Produktionsmodus**.
3. Tab **Regeln**: den Inhalt durch Folgendes ersetzen → **Veröffentlichen**:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /notebooks/{nb} {
         allow get, create, update: if true;
         match /pages/{page} {
           allow read, create, update: if true;
         }
       }
     }
   }
   ```

4. **Projektübersicht** (Zahnrad → Projekteinstellungen) → unten **App hinzufügen → Web (`</>`)** → Name eingeben → registrieren.
5. Den angezeigten `firebaseConfig`-Block in `index.html` bei `const FIREBASE_CONFIG = null;` eintragen, z. B.:

   ```js
   const FIREBASE_CONFIG = {
     apiKey: "…", authDomain: "ink-notes-xxxx.firebaseapp.com", projectId: "ink-notes-xxxx",
     storageBucket: "…", messagingSenderId: "…", appId: "…",
   };
   ```

   Diese Werte sind nicht geheim, sie sind dafür gedacht, in Webseiten zu stehen.
6. Committen und pushen. Rechts oben in der Leiste zeigt ein Punkt den Status: grün = synchronisiert, orange = wird gespeichert bzw. offline, rot = Fehler, grau = nur lokal.

Beim ersten Öffnen eines Notizbuchs mit Sync werden die bisher lokal gespeicherten Notizen dieses Browsers automatisch hochgeladen.

**Sicherheit:** Es gibt keinen Login. Wer die Notizbuch-Id kennt, kann es lesen und bearbeiten, und eine Liste aller Notizbücher kann niemand abrufen. Nimm darum zufällige Ids (⋯ → *Neues Notizbuch / Embed-Link*) statt `?id=mathe`.

Die kostenlose Stufe (50.000 Lesevorgänge und 20.000 Schreibvorgänge pro Tag) reicht für den persönlichen Gebrauch locker: Ein Strich ist ein Schreibvorgang, eine Seite laden ist ein Lesevorgang.

## Bedienung

| Aktion | Eingabe |
| --- | --- |
| Stift / Textmarker / Radierer | `P` / `H` / `E` |
| Rückgängig / Wiederholen | `⌘Z` / `⇧⌘Z` (Windows: `Strg`) |
| Zoom | `⌘+` / `⌘−`, Trackpad-Pinch, Klick auf % = einpassen |
| iPad / Touch | ein Finger scrollt, zwei Finger zoomen, nur Apple Pencil (bzw. Maus) schreibt |
