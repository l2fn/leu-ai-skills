# Karten und externe Inhalte

Wenn Karten, eingebettete Drittinhalte oder externe Widgets eingefügt oder geändert werden sollen, gehe standardmäßig datensparsam vor.

## Standardregel

- Wenn nicht ausdrücklich anders beschrieben, nutze für externe Inhalte immer einen **Consent-Wrapper**.
- In diesem Projekt wird das in der Regel über `nte-consent-blocker` umgesetzt.
- Binde externe `iframe`-Inhalte nicht ungeschützt direkt ein, wenn kein ausdrücklicher Wunsch dagegen besteht.

Typische Fälle:

- Google Maps
- eingebettete Videos
- externe Termin- oder Formular-Widgets
- Badges, Zertifikate oder externe Informations-Widgets

## Projektmuster

Orientiere dich an bestehenden Stellen wie `./docs/kontakt/index.md`.

### Einzelner Standort mit Consent-Wrapper

```html
<style>
  .google-maps-consent {
    --default-template: '<iframe src="{{site.data.general.map_url}}"></iframe>';
  }
</style>

<nte-consent-blocker class="google-maps-consent" slot="top"></nte-consent-blocker>
```

Für **einzelne Standorte** soll bevorzugt das Einbinden über `--default-template` genutzt werden.

## Mehrere Karten auf einer Seite

Wenn mehrere Karten auf derselben Seite eingebunden werden, braucht jede Karte ihren **eigenen Consent-Blocker** und in der Praxis meist auch eine **eigene CSS-Klasse** mit eigener `--default-template`-Definition.

Beispiel mit zwei Karten:

```html
<style>
  .map-meoclinic-consent {
    --default-template: '<iframe src="{{ site.data.general.map_url }}"></iframe>';
  }

  .map-westend-consent {
    --default-template: '<iframe src="https://www.google.com/maps/embed?pb=..."></iframe>';
  }
</style>

<nte-consent-blocker class="map-meoclinic-consent" slot="top"></nte-consent-blocker>
<nte-consent-blocker class="map-westend-consent" slot="top"></nte-consent-blocker>
```

## Consent-Wrapper mit eigenem `<template>`

Statt `--default-template` kann der Inhalt auch direkt als `<template>` im `nte-consent-blocker` hinterlegt werden. Das ist besonders sinnvoll, wenn das eingebettete Element mehr Attribute braucht, mehrzeilig gepflegt werden soll oder der Inhalt im Markdown klarer lesbar sein soll.

Für **einzelne Standorte** ist trotzdem `--default-template` das bevorzugte Muster. Ein direktes `<template>` ist vor allem dann sinnvoll, wenn mehrere Attribute, komplexeres Markup oder besser lesbares HTML benötigt werden.

### Mehrere Karten mit jeweils eigenem `<template>`

Wenn mehrere Karten auf derselben Seite eingebunden werden, bekommt jede Karte ihren **eigenen Consent-Blocker** mit eigenem `<template>`.

```html
<nte-consent-blocker slot="top">
  <template>
    <iframe
      src="{{ site.data.general.map_url }}"
      title="Karte MEOCLINIC Berlin Mitte"
      loading="lazy"
      referrerpolicy="no-referrer-when-downgrade"
      class="w-100"
      style="min-height: 320px"
    ></iframe>
  </template>
</nte-consent-blocker>

<nte-consent-blocker slot="top">
  <template>
    <iframe
      src="https://www.google.com/maps/embed?pb=..."
      title="Karte DRK Kliniken Berlin Westend"
      loading="lazy"
      referrerpolicy="no-referrer-when-downgrade"
      class="w-100"
      style="min-height: 320px"
    ></iframe>
  </template>
</nte-consent-blocker>
```

Dieses Muster ist die bevorzugte Alternative, wenn eine direkte Template-Einbindung klarer ist als eine CSS-basierte `--default-template`-Definition, besonders bei mehreren Attributen oder komplexerer Einbettung.

## Arbeitsregel für den Skill

- Prüfe bei Karten und externen Inhalten zuerst, ob bereits ein bestehendes Muster im Projekt vorhanden ist.
- Verwende möglichst das bestehende Projektmuster statt einer neuen freien HTML-Einbindung.
- Wenn der Nutzer nur „Karte einbauen“ oder „externen Inhalt einbetten“ sagt, nutze standardmäßig den Consent-Wrapper.
- Für einzelne Standorte bevorzuge `--default-template`.
- Wenn externe Inhalte mehr Attribute, mehrzeilige Struktur oder besser lesbares HTML brauchen, nutze ein direktes `<template>` im Consent-Wrapper.
- Wenn der Nutzer ausdrücklich ein anderes Muster vorgibt, folge dieser Vorgabe, solange die Änderung innerhalb von `./docs/` bleibt.
- Wenn zusätzliche Datenschutztexte, neue Skripte oder Änderungen außerhalb von `./docs/` nötig wären, weise kurz auf die fehlende Voraussetzung hin, statt außerhalb des erlaubten Scopes zu arbeiten.
