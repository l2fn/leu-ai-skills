# Vorlage für `customer-edit-request.md`

```markdown
# Customer Edit Request

- Datum: YYYY-MM-DD
- Skill: `edit-website-by-email`
- Scope: Analyse der Markdown-Dateien unter `./docs/`
- Ergebnis: Keine Änderungen wurden ausgeführt

## Original E-Mail mit Kommentaren

> Sehr geehrtees Team,
>
> ich habe einige Anmerkungen zu unserer Website. Bitte prüfen Sie die folgenden Punkte:
>
> Mein Name ist nicht korrekt geschrieben. Es sollte "Dr. med. Ulrich " heißen, nicht "Dr. med Ulrich ".

Fix: In den Dateien `docs/pages/index.de.md` und `docs/leistungen/hausarzt-uebersicht.de.md` wurde der Name von "Dr. med Ulrich " auf "Dr. med. Ulrich " korrigiert.

> Bitte Bilder austauschen. Ich habe Ihnen diese Angehängt

Todo: Bilder in Mediastore bereitstellen

> Die Farbe des Buttons auf der Startseite in Blau ändern

Todo: Css anpassen

> Außerdem soll das und das wie Besprochen gemacht werden

Ask: Was wurde besprochen?

> Vielen Dank
>   XYZ



```

## Hinweise

- Die Original-E-Mail immer vollständig zitieren.
- Alle Änderungswünsche direkt an der passenden Stelle in der zitierten E-Mail kommentieren.
- Nur Kommentare mit `Fix:`, `Todo:`, `Ask:` oder `Duplicate:` verwenden.
- Keine zusätzlichen Sections, Aufgabenlisten, Fragenblöcke oder Zusammenfassungen ergänzen.
- Wenn ein Wunsch eher in Includes, Layouts oder Assets liegen könnte, dies direkt im passenden Kommentar vermerken, aber trotzdem keine Änderungen ausführen.
