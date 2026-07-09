---
name: upgrade-website
description: Bringt eine Website unter ./docs sowie das Repo (package.json, package-lock, Vite-Konfiguration, Theme-Dateien) auf den aktuellen Stand von themejs2 und übernimmt bestehende Individualisierungen gezielt in die neue Basis.
allowed-tools: read bash edit write
---

# Upgrade Website

Verwende diesen Skill, wenn ein bestehendes Website-Repo technisch und strukturell auf den aktuellen Stand von `themejs2` gebracht werden soll.

## Ziel

Der Skill aktualisiert:

- die Website-Dateien unter `./docs/`
- die Theme-Struktur, vor allem `/_layouts/*` und `/_includes/*`
- die Paketstände und Lockfiles des Repos
- `vite.config.*` und angrenzende Build-Konfiguration
- bei Bedarf `docs/_config.yml` sowie `docs/_src/*`

Dabei gilt:

- **zuerst** die aktuelle `themejs2`-Basis per npm installieren bzw. auflösen
- **danach** die Konfiguration aus dem installierten `themejs2`-Projekt mit dem bestehenden Projekt vergleichen
- **anschließend** projektindividuelle Anpassungen aus der alten Version **gezielt** in die neue Basis übernehmen
- **keine** blinde 1:1-Überschreibung, wenn dadurch Branding, Navigation, Inhalte oder kundenspezifische Logik verloren gehen würden

## Optional:

Wenn ein alte inhalte verzeichnis angegeben ist, übernimm die Inhalte aus dem alten Verzeichnis sowie in individualisierungen und
baue die neue Version unter ./docs/ auf.

## Wichtige Pfade

Prüfe zuerst, welche dieser Pfade im Projekt existieren:

- `./docs/`

Verwende danach den tatsächlich vorhandenen Website-Root. Wenn beide existieren, arbeite mit dem Pfad, der bereits die aktive Jekyll-/Website-Struktur enthält.

Besonders wichtige Dateien und Ordner:

- `package.json`
- `package-lock.json`
- `vite.config.ts` oder `vite.config.js`
- `<site-root>/_layouts/*`
- `<site-root>/_includes/*`
- `<site-root>/_config.yml`
- `<site-root>/_src/*`
- optional als Theme-Referenz:
  - `./workspaces/themejs2/`
  - `./node_modules/@leuffen/themejs2/`

## Arbeitsablauf

1. **Projektstruktur erfassen**
   - Ermittle zuerst die relevanten Dateien und Ordner mit `find`/`rg`.
   - Lies mindestens:
     - `package.json`
     - `vite.config.*`
     - die wichtigsten Layouts/Includes
     - falls vorhanden: `workspaces/themejs2/package.json`, `workspaces/themejs2/vite.config.*`

2. **themejs2 aktualisieren**
   - Versuche zuerst, die aktuelle Version per npm zu installieren:
     ```bash
     npm install @leuffen/themejs2@latest
     ```
   - Wenn das Paket nicht über die Registry verfügbar ist, aber bereits als Workspace oder lokales Paket im Repo existiert, verwende die vorhandene installierte/local-workspace-Version als **maßgebliche aktuelle Basis** und dokumentiere das kurz in der Antwort.
   - Wenn es sowohl eine Registry-Version als auch eine lokale Workspace-Version gibt, nutze die tatsächlich installierte Version unter `node_modules/@leuffen/themejs2/` als Vergleichsbasis.

3. **Theme-Basis ermitteln**
   - Vergleiche das Projekt mit der Theme-Basis, insbesondere:
     - `<site-root>/_layouts/*`
     - `<site-root>/_includes/*`
     - `<site-root>/_src/*`
     - `<site-root>/_config.yml`
     - `vite.config.*`
   - Nutze dafür bevorzugt `diff -ru`, `find` und gezieltes Einlesen mit `read`.

4. **Individualisierungen erkennen**
   - Identifiziere projektspezifische Anpassungen wie z. B.:
     - Branding, Logos, Farben, Texte
     - Navigation und Footer-Struktur
     - kundenspezifische Includes oder Teil-Layouts
     - abweichende Jekyll-Konfiguration
     - zusätzliche Build-Hooks, Proxy-Regeln oder Alias-Konfiguration in Vite
   - Behandle solche Unterschiede **nicht automatisch als Altlasten**.
   - Übernimm nur das, was erkennbar projektspezifisch oder funktional notwendig ist.

5. **Neue Basis übernehmen**
   - Übernimm fehlende oder modernisierte Dateien/Strukturen aus `themejs2` in das Projekt.
   - Aktualisiere bestehende Dateien so, dass sie möglichst nah an der neuen Basis liegen.
   - Migriere Individualisierungen anschließend sauber in diese neue Struktur.
   - Achte besonders auf diese Bereiche:
     - `default.html`
     - `website.html`
     - Layout-Ketten wie `0_blanc.html`, `1_body.html`, `2_script.html`, `3_main.html`
     - projektspezifische Varianten unter `_layouts/page/` oder `_layouts/legal/`
     - Includes unter `_includes/el/`, `_includes/do/`, `_includes/_styles/`, `_includes/dist/`

6. **Repo- und Build-Konfiguration modernisieren**
   - Prüfe und aktualisiere bei Bedarf:
     - `package.json`
     - `package-lock.json`
     - `vite.config.*`
   - Übernimm sinnvolle Verbesserungen aus der neuen Theme-Basis, z. B.:
     - Alias-Konfiguration
     - Build-Input/Output
     - Copy-Hooks für Jekyll-Assets
     - Dev-Server-/Proxy-Konfiguration
   - Entferne bestehende Sonderlogik **nur dann**, wenn sie nachweislich obsolet ist oder von der neuen Basis sauber ersetzt wird.

7. **Build validieren**
   - Führe nach den Änderungen nach Möglichkeit mindestens einen Build- oder Installationsschritt aus, z. B.:
     ```bash
     npm install
     npm run build
     ```
   - Wenn ein Build fehlschlägt, behebe klare, direkt durch das Upgrade verursachte Probleme.
   - Wenn der Fehler unabhängig vom Upgrade wirkt oder externe Voraussetzungen fehlen, ändere nicht blind weiter, sondern dokumentiere das knapp in der Antwort.

## Entscheidungsregeln

- **Bevorzuge die neue Theme-Basis** bei strukturellen oder technischen Fragen.
- **Bevorzuge das bestehende Projekt** bei kundenspezifischen Inhalten, Texten, Navigation, Markenbestandteilen und bewusst abweichenden Layout-Details.
- Wenn eine Datei im Projekt zusätzliche projektbezogene Blöcke enthält, diese **in die neue Datei hineinmigrieren**, statt einfach die alte Datei komplett stehen zu lassen.
- Wenn alte Dateien offensichtlich in mehrere neue Dateien aufgeteilt wurden, übertrage die Individualisierung in die neue Aufteilung.
- Wenn neue Theme-Dateien im Projekt fehlen, lege sie an.
- Entferne alte Dateien nur dann, wenn klar ist, dass sie von der neuen Struktur ersetzt wurden und im Projekt nicht mehr benötigt werden.

## Was nicht passieren darf

- Keine unreflektierte Komplettkopie, die kundenspezifische Anpassungen löscht.
- Keine Änderungen in `node_modules/` direkt editieren.
- `workspaces/themejs2/` nur als Referenz oder installierte Quelle behandeln, nicht als Ziel der Website-Migration, außer der Nutzer fordert explizit Änderungen dort an.
- Keine Built-Artefakte manuell „erfinden“. Generierte Dateien nur aktualisieren, wenn sie durch den normalen Build entstehen.
- Keine beliebigen Refactorings außerhalb des Upgrade-Ziels.

## Praktische Hinweise

- Nutze für die Vergleichsanalyse bevorzugt:
  ```bash
  diff -ru <site-root>/_layouts <theme-root>/docs/_layouts || true
  diff -ru <site-root>/_includes <theme-root>/docs/_includes || true
  ```
- Typische Theme-Root-Kandidaten sind:
  - `./node_modules/@leuffen/themejs2`
  - `./workspaces/themejs2`
- Lies nur die tatsächlich betroffenen Dateien vollständig ein, bevor du editierst.
- Bei mehreren Änderungen in derselben Datei: in einem `edit`-Aufruf bündeln.

## Antwortformat

Antworte am Ende kurz mit:

- aktualisierten Dateien
- übernommenen Individualisierungen
- ggf. bewusst nicht übernommenen Altteilen
- Ergebnis von `npm install` / `npm run build`
- offenen Punkten oder Blockern


## Do's
- Entferne die folgenden keys als allen frontmatter dateien: "use_header", "use_footer"
- 
