---
name: jekyll-page
description: Konfigurationshinweise und Regeln für die Bearbeitung von Jekyll-Seiten. (Markdown-Dateien unter `./docs/` mit YAML-Frontmatter)
allowed-tools: read bash edit write
---

# Jekyll Page

Die Jekyll-Seiten starten unter `./docs/` und sind Markdown-Dateien mit YAML-Frontmatter.

## Aufteilung

Footer und Header ligen unter `./docs/_includes/` und werden von Jekyll automatisch in die Seiten eingebunden.

Die Markdown-Dateien nutzen die Layouts unter `./docs/_layouts/`.

## HTML/CSS Konventionen

- Bei HTML in Layouts und Includes nach Möglichkeit bestehende Utility-Klassen des Projekts verwenden (z.B. `d-flex`, `flex-*`, `gap-*`, `m-*`, `p-*`, `text-*`, `justify-content-*`, `align-items-*`).
- Für Abstände bevorzugt die vorhandenen Margin-/Padding-Utilities (`m-*`, `mt-*`, `mb-*`, `ms-*`, `me-*`, `p-*`, usw.) nutzen, auch wenn die Abstände nicht zu 100% exakt sind.
- Für responsive Klassen die trunkjs/responsive Notation verwenden, z.B. `xl:col-5` oder `-xl:col-12`, statt Bootstrap-Breakpoint-Klassen wie `col-md-*`, `col-lg-*`, `col-xl-*`.
- Bedeutung der Prefixe allgemein: `xl:<class>` bedeutet, dass `<class>` ab Breakpoint `xl` gilt. `-xl:<class>` bedeutet, dass `<class>` unterhalb von `xl` gilt.
- Bootstrap-Breakpoint-Notation gilt in diesem Projekt als ersetzt durch die trunkjs/responsive Syntax.
- Neue `<style>`-Blöcke oder eigene CSS-Klassen nur in Ausnahmefällen anlegen, z.B. wenn Sub-Elemente selektiert werden müssen, Utilities nicht ausreichen oder ein wiederverwendbares Styling wirklich nötig ist.
- Wenn in einem `<style>`-Block eigene CSS-Variablen definiert werden, diese gesammelt am Anfang des Style-Blocks bzw. am Anfang des Top-Level-Selectors definieren, bevor andere Regeln diese verwenden.
- Vor neuen Klassen immer prüfen, ob eine bestehende Struktur in `./docs/_includes/` oder `./docs/_layouts/` als Vorlage wiederverwendet werden kann.

## Gliederung der Markdown-Dateien

Startseite unter `./docs/index.md`.

### Kategorie-Seiten

Kategorie-Seiten liegen in einem Unterverzeichnis unter `./docs/`.

z.B. `./docs/leistungen/index.md` für die Kategorie "Leistungen". Diese werden als Dropdown-Feld angezeigt.

Unter Kategorie-Seiten kann es beliebig viele unterseiten geben, z.B. `./docs/leistungen/hausarzt-uebersicht.md` für die Unterseite "Hausarzt Übersicht".

Auch weitere Kategorie-Unterseiten sind möglich, z.B. `./docs/leistungen/hausarzt-uebersicht/index.md` für die Unterseite "Unterseite" unter "Hausarzt Übersicht".


### Leaf-Seiten

Unter Kategorie-Seiten kann es beliebig viele unterseiten geben, z.B. `./docs/leistungen/hausarzt-uebersicht.md` für die Unterseite "Hausarzt Übersicht".

Diese werden in der Hauptnavigation als normaler Punkt angezeigt. Leaf-Seien von Kategorie-Seiten werden in der Navigation als Dropdown Elemenet im Dorpdown des Kategie-Felds angezeigt.

### Navigationsleisten

über die Frontmatter-Variable

```
ptags:
- nav
```

Kann die Seite in den Navigationsbaum aufgenommen werden. Die Reihenfolge der Navigation wird über die Frontmatter-Variable `order` gesteuert.

Es können ein oder mehrere ptags angegeben werden. Definierte Ptags sind:

```
nav -> Hauptnavigation mit unternavigation
footer -> Footer Navigation mit unterseiten
subfooter -> Flache Navigation für z.B. Impressum, Datenschutz, AGB, etc. (keine Unterelemente)
```

Standardmässig wird `title` in der navigation angezeigt. Falls `nav_title` angegeben ist, wird dieser name in der Navigation angezeigt. 

## Metadaten

```
title: <Titel der Seite> (wird in der Navigation und im Browser-Tab angezeigt)
nav_title: <Titel der Seite in der Navigation> (optional, falls abweichend von title)
description: <Beschreibung der Seite> (optional, wird in der Meta-Description im HTML-Header verwendet)
permalink: <URL-Pfad der Seite> (optional, falls abweichend vom Dateipfad) - beibehalten, wenn gesetzt - ansonsten nur bei aufforderung setzen
```

## Navigationsstruktur

```
./docs/
    - index.md (Startseite)
    - leistungen/
      - index.md (Kategorie-Seite "Leistungen" - Sollte per default `layout: index` haben)
      - diagnostik.md (Leaf-Seite "Diagnostik" - layout: default)
    - kontakt.md (Leaf-Seite "Kontakt" - layout: default)
```

Vergebe beim neue anlegen von Seiten keinen permalink, sondern lass Jekyll den Pfad automatisch generieren. 

Achtung: Wenn du index-Seiten anlegst: immer layout: index verwenden - dieser fügt automatisch links zu allen unterseiten ein. (außer bei der Startseite `./docs/index.md` - Dies ist die Hauptseite)

## Obsolete Frontmatter-Variablen

```
pid: 
lang:
short_title: -> ersetzen durch nav_title nur wenn abweichend von title
```

