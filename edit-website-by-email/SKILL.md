---
name: edit-website-by-email
description: Analysiert Kunden-E-Mails mit Änderungswünschen für diese Website. Prüft die Markdown-Dateien unter ./docs/, bewertet jeden Wunsch direkt im Kontext der zitierten Original-E-Mail und erstellt ./customer-edit-request.md ausschließlich nach den Sections aus references/report-template.md. Führt keine Website-Änderungen aus.
allowed-tools: read bash write edit
---

# Edit Website by Email

Verwende diesen Skill, wenn ein Kunde per E-Mail Änderungswünsche für die Website sendet und zunächst **nur eine Umsetzungsanalyse** erstellt werden soll.

## Ziel

Erstelle genau **eine Arbeitsdatei** im Projektwurzelverzeichnis (oder überschreibe diese, wenn vorhanden:

- `./customer-edit-request.md`

Diese Datei enthält ausschließlich die Struktur aus `references/report-template.md`:

- den Header
- die Section `## Original E-Mail mit Kommentaren`
- die vollständige Original-E-Mail als Blockquote
- alle Bewertungen direkt an den passenden Stellen zur zitierten E-Mail

## Wichtige Regeln

- **Niemals** Änderungen an Dateien unter `./docs/` oder anderswo ausführen.
- **Niemals** Inhalte in Website-Dateien umschreiben, löschen oder committen.
- Der Skill ist **rein analytisch** und erstellt nur die Planungsdatei `./customer-edit-request.md`.
- Wenn die E-Mail nicht im Prompt enthalten ist, bitte den Nutzer um die vollständige E-Mail.
- Falls Anforderungen vermutlich nur außerhalb von `./docs/*.md` lösbar wären, vermerke das klar als Einschränkung.
- Nutze standardmäßig **keine interaktiven Rückfragen**.
- Erzeuge **keine zusätzlichen Sections** oder Überschriften in `./customer-edit-request.md`.
- Schreibe **keine separate Aufgabenliste**, **keinen separaten Fragenblock** und **keine separate Zusammenfassung** in die Datei.
- Offene Punkte, Rückfragen und Einschränkungen gehören als `Ask:`- oder `Todo:`-Kommentare **direkt an die passende Stelle in der zitierten E-Mail**.

## Arbeitsablauf

1. **E-Mail erfassen**
   - Nimm die E-Mail exakt so, wie sie vom Nutzer bereitgestellt wurde.
   - Zerlege sie in einzelne Änderungswünsche oder Aufgaben.
   - Fasse ähnliche Punkte nur zusammen, wenn das eindeutig ist.

2. **Markdown-Dateien unter `./docs/` prüfen**
   - Ermittle zuerst alle Markdown-Dateien:
     ```bash
     find ./docs -type f -name '*.md|html|yaml' | sort
     ```
   - Prüfe den vorhandenen Inhalt dieser Dateien auf passende Seiten, Abschnitte, Themen oder Formulierungen.
   - Lies die relevanten Dateien im Detail mit dem `read`-Tool.
   - Halte in der Ausgabedatei fest, welche Dateien geprüft wurden und welche davon relevant erscheinen.

3. **Umsetzbarkeit pro Task bewerten**
   - Ordne jeden Wunsch einem der folgenden Status zu:
        - **Fix**: Die Änderung ist klar und kann direkt in den gefundenen Dateien umgesetzt werden.
        - **Todo**: Die Änderung ist klar, aber es ist unklar, wo sie umgesetzt werden soll (z.B. in Includes, Layouts, Assets).
        - **Ask**: Die Änderung ist unklar oder widersprüchlich. Es müssen Rückfragen gestellt werden.
        - **Duplicate**: Die Änderung wurde bereits mit einem anderen Task erledigt (z. B. globale Änderungen von Farben, Namen oder E-Mail-Adressen).
   - Schreibe die Bewertung **direkt nach der passenden zitierten Passage** der E-Mail.
   - Jeder Kommentar beginnt mit `Fix:`, `Todo:`, `Ask:` oder `Duplicate:`.

4. **Planungsdatei erstellen**
   - Schreibe `./customer-edit-request.md` neu.
   - Verwende **nur** das Format und die vorhandenen Sections aus [references/report-template.md](references/report-template.md).
   - Achte darauf, dass die Original-E-Mail vollständig als Blockquote enthalten ist.
   - Kommentiere **alle Änderungswünsche direkt im quoted mail**, statt zusätzliche Abschnitte zu erzeugen.
   - Ergänze keine weiteren Überschriften, Listen oder Meta-Sections außerhalb der Vorlage.

5. **Abschluss**
   - Stelle in der Datei ausschließlich über den Header `- Ergebnis: Keine Änderungen wurden ausgeführt` klar, dass **keine Änderungen umgesetzt** wurden.
   - Biete optional in deiner Antwort an den Nutzer an, im nächsten Schritt auf Basis der Datei die tatsächlichen Änderungen auszuführen.


## Du darfst

- relevante Markdown-, HTML- und YAML-Dateien unter `./docs/` lesen und für die Analyse heranziehen
- potenziell betroffene Dateien konkret benennen
- notwendige Rückfragen, Einschränkungen und Umsetzungsorte als Kommentare in der zitierten E-Mail festhalten


## Du darfst nicht

- eingene Logikprogrammierung machen
- Änderungen an Layouts, Includes oder Assets vornehmen.
- Irgendwelchen Python Code erstellen, um dateien einzulesen. Nutze die normale file read funktionaliät.


## Wichtig

- Bilder liegen immer im Mediastore, auf den du keinen Zugriff hast. Lege eine Todo aktion an, wenn auf Bilder verwiesen wird, die nicht in den Markdown-Dateien liegen.
- Farben, Schriftarten etc. werden zentral in CSS Dateien gesteuert. Wenn ein Kunde an mehreren Stellen die Änderung der Farbe von z.B. einem Button vorschlägt, gehe nur beim erten mal darauf ein. Nimm danach an, dass die Änderung global ausgeführt wrude.
- Captschas werden nicht benötigt
- Fasse dich Kurz bei der Beschreibung der Änderungen. schreibe z.B. `datei xyz.md: ersetze 'abcd' durch  'efgh'` oder `datei xyz.md: ändere 'abcd' in 'efgh'`


## Anforderungen an die Analyse

- Arbeite **dateibezogen und konkret**. Vermeide pauschale Aussagen wie „muss irgendwo auf der Website geändert werden“.
- Achte darauf, wenn Änderungen vermutlich in YAML-Dateien, Includes, Layouts oder Assets liegen würden, und vermerke das als Einschränkung im passenden Kommentar.
- Wenn ein Task mehrere Dateien betrifft, nenne alle betroffenen Dateien im zugehörigen Kommentar.
- Wenn eine Anforderung nicht eindeutig ist, formuliere daraus präzise Rückfragen direkt an der passenden Stelle in der zitierten E-Mail.
- Wenn die gewünschte Information bereits in einer Datei existiert, notiere das im zugehörigen Kommentar und beschreibe die mutmaßliche Anpassung.
- Wenn du keinen passenden Inhalt in `./docs/*.md` findest, benenne das ausdrücklich im zugehörigen Kommentar.
- Füge keine erfundenen Inhalte oder Annahmen als beschlossene Änderungen ein.
- Die Ausgabedatei darf keine weiteren Sections enthalten als die aus `references/report-template.md`.

