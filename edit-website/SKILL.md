---
name: edit-website
description: Setzt Änderungen an der Website um. Liest `./customer-edit-request.md` oder Änderungswünsche im Prompt ein und ändert Dateien in `./docs/**/*.md|html|yml|yaml` um.
allowed-tools: read bash edit write
---

# Edit Website

Nutze diesen Skill, wenn `./customer-edit-request.md` bereits existiert und die dort markierten Website-Änderungen umgesetzt werden sollen.

Wenn keine customer-edit-request.md existiert, erwartetst Du die Änderungen im Prompt. Wenn keine Änderungen angegeben sind,
zeige eine Fehlermeldung an und beende den Skill.

## Regeln

- Prüfe zuerst, ob `./customer-edit-request.md` existiert.


### Wenn `./customer-edit-request.md` existiert:

#### Regeln:

- Falls die Datei fehlt: keine Änderungen ausführen und den Nutzer kurz darauf hinweisen, dass sie zuerst mit dem Skill `edit-website-by-email` erzeugt werden muss.
- Verwende **nur** die unzitierten Kommentarzeilen aus `./customer-edit-request.md` als Aufgabenquelle.
- Alles im Blockquote (`> ...`) ist nur Kontext und wird nie direkt umgesetzt.
- `Fix:`-Einträge führst du aus, wenn die Änderung konkret ist und nur Dateien unter `./docs/` mit den Endungen `.md`, `.html`, `.yml` oder `.yaml` betrifft.
- `Todo:`-Einträge führst du **nur dann** aus, wenn daraus trotzdem eine vollständig konkrete Änderung an solchen Dateien unter `./docs/` eindeutig ableitbar ist. Sonst überspringst du sie.
- `Ask:`, `Duplicate:` und unmarkierte Textstellen werden übersprungen.
- `./customer-edit-request.md` selbst bleibt unverändert.

#### Arbeitsablauf

1. Lies `./customer-edit-request.md` vollständig.
2. Sammle alle umsetzbaren `Fix:`-Einträge und nur die wirklich konkreten `Todo:`-Einträge.
3. Ermittle die betroffenen Dateien unter `./docs/`, z. B. mit:
   ```bash
   find ./docs -type f \( -name '*.md' -o -name '*.html' -o -name '*.yml' -o -name '*.yaml' \) | sort
   ```
4. Lies nur die tatsächlich betroffenen Dateien.
5. Setze die Änderungen präzise um.
    - Nutze kleine, gezielte Edits.
    - Wenn mehrere Änderungen dieselbe Datei betreffen, bündele sie nach Möglichkeit in einem `edit`-Aufruf.
    - Lege neue Dateien unter `./docs/` nur an, wenn ein `Fix:` oder ausführbarer `Todo:` den Inhalt ausreichend klar vorgibt.
6. Überspringe Aufgaben, die unklar sind oder zusätzliche Assets/Logik außerhalb des erlaubten Scopes brauchen.
7. Antworte kurz mit:
    - geänderten Dateien
    - bewusst übersprungenen Aufgaben
    - ggf. fehlenden Voraussetzungen

#### Wichtig

- Wenn ein Eintrag nur sagt, dass etwas „wahrscheinlich“ in CSS, Formularlogik, Assets oder externen Dateien liegt, wird er nicht umgesetzt.
- Wenn ein `Todo:` nur eine offene Beschaffung beschreibt (z. B. fehlende Bilder oder Logos), wird er nicht umgesetzt.
- Wenn eine Änderung im Report bereits als erledigt oder doppelt markiert ist, wird sie nicht erneut umgesetzt.
- Arbeite immer dateibezogen und ohne zusätzliche Interpretation jenseits des Reports.


### Wenn Änderungen im Prompt angegeben sind:

- Führe diese in den Dateien unter `./docs/` mit den Endungen `.md`, `.html`, `.yml` oder `.yaml` aus.
- 


## Globale Regeln

- Änderungen außerhalb von `./docs/` sind verboten.
- Ändere keine CSS-, JS-, Bild-, PDF-, Backend- oder Mediastore-Dateien.
- Wenn gesagt wird, dass Du Bilder oder Datein ändern sollst: Es sollten dir im Prompt die neuen Dateien angeben sein. Ordne diese
  anhand ihres Dateinamens oder der Beschreibung im Alt-Text den ensprechenden Stellen zu. Ändere diese nur dort, wo es explizit passt.
  Gib am Ende eine Liste aus, welche Datei wo verlinkt wurde.
