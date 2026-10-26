# Ink Notes

Handschriftliche Notizen im Stil von GoodNotes/OneNote – als einzelne HTML-Seite, gehostet über GitHub Pages und in Notion einbettbar.

**Funktionen:** Stift mit Druckempfindlichkeit (Apple Pencil / Stylus), Textmarker, Radierer (auch über die Stylus-Radiertaste), Farben, drei Strichstärken, liniertes / kariertes / gepunktetes / leeres Papier, mehrere A4-Seiten, Rückgängig/Wiederholen, Zoom, „Nur Stift“-Modus (Finger scrollen, Stift schreibt), Export als PNG oder PDF, Backup als JSON.

## 1. Auf GitHub Pages veröffentlichen

1. Auf github.com ein neues **öffentliches** Repository anlegen, z. B. `ink-notes`.
2. `index.html` und `.nojekyll` hochladen (*Add file → Upload files*), committen.
3. *Settings → Pages → Build and deployment*: Source **Deploy from a branch**, Branch **main**, Ordner **/ (root)** → *Save*.
4. Nach ca. einer Minute ist die Seite erreichbar unter
   `https://<dein-github-name>.github.io/ink-notes/`

Oder per Terminal:

```bash
git init && git add index.html .nojekyll README.md && git commit -m "Ink Notes"
git branch -M main
git remote add origin https://github.com/<dein-github-name>/ink-notes.git
git push -u origin main
```

## 2. In Notion einbetten

1. In Notion `/embed` tippen, den Link einfügen:
   `https://<dein-github-name>.github.io/ink-notes/?id=mathe`
2. Den Embed-Block unten am Rand auf die gewünschte Höhe ziehen.

**Jede `?id=` ist ein eigenes Notizbuch.** Für jede Notion-Seite eine andere id verwenden (`?id=mathe`, `?id=meeting-2026-10-26`, …). Im Menü (⋯) → *Neues Notizbuch / Embed-Link* erzeugt einen zufälligen Link zum Kopieren.

Optional: `&theme=dark` oder `&theme=light` erzwingt das Farbschema der Werkzeugleiste.

## Wo werden die Notizen gespeichert?

Lokal im Browser (IndexedDB) des jeweiligen Geräts – es gibt keinen Server und keine Synchronisation.

- iPad und Laptop sehen also **nicht** dieselben Notizen.
- Browserdaten löschen löscht auch die Notizen. Safari kann Daten von eingebetteten Seiten nach längerer Nichtnutzung entfernen.
- Deshalb regelmäßig **⋯ → Backup exportieren** nutzen; über *Backup importieren* lässt sich alles wiederherstellen (auch auf einem anderen Gerät).

## Bedienung

| Aktion | Eingabe |
| --- | --- |
| Stift / Textmarker / Radierer | `P` / `H` / `E` |
| Rückgängig / Wiederholen | `⌘Z` / `⇧⌘Z` (Windows: `Strg`) |
| Zoom | `⌘+` / `⌘−`, Trackpad-Pinch, Klick auf % = einpassen |
| Mit Fingern scrollen statt schreiben | Hand-Symbol („Nur Stift“) |
