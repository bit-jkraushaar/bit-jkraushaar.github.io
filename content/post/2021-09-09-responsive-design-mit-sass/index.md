---
date: "2021-09-09T15:30:00Z"
title: Responsive Webdesign mit SASS
description: "In diesem Artikel wird erläutert, wie SASS genutzt werden kann, um Responsive Webdesign zu vereinfachen. Es werden fünf Schritte beschrieben: die Definition der maximalen Breiten für verschiedene Gerätegrößen, das Erstellen einer Funktion zur Ermittlung der maximalen Breite basierend auf einem Breakpoint, die Definition von Mixins mit Media Queries für unterschiedliche Bildschirmgrößen, die Schaffung eines Mixins für das Responsive Design, und schließlich die Anwendung dieses Mixins. Der Artikel betont, dass das Beispiel konstruiert ist und in der Praxis einfachere Methoden existieren könnten, zielt aber darauf ab, das grundlegende Prinzip zu vermitteln. Zudem wird erwähnt, dass das vorgestellte Verfahren durch den Einsatz von Schleifen wie @for oder @each weiter optimiert werden kann."
image: header.png
---

Mithilfe von [SASS](https://sass-lang.com) lässt sich das Responsive Webdesign deutlich vereinfachen.
Als Beispiel wird die maximale Breite des dargestellten Seiteninhalts für drei Größen definiert.
Das Beispiel ist dabei natürlich konstruiert und in der Praxis gäbe es einfacherer Methoden, das gewünschte Ergebnis zu erreichen.
Hier soll vorallem das zu Grunde liegende Prinzip verdeutlicht werden.

# Schritt 1: Definition der maximalen Breiten

Die maximalen Breiten werden als Liste definiert:

```css
$max-widths: (300px 600px 900px);
```

# Schritt 2: Definition einer Function für die maximale Breite

Als nächstes schreiben wir eine Function, die die maximale Breite abhängig von einem Breakpoint ermittelt.
Der Breakpoint ist dabei nur ein Index in der zuvor angelegten Liste:

```css
@function max-width($breakpoint) {
    @return list.nth($max-widths, $breakpoint);
} 
```

Damit `list.nth` genutzt werden kann, muss zudem folgender Import am Anfang der Datei hinzugefügt werden: `@use 'sass:list';`

# Schritt 3: Definition von Mixins mit Media Queries

Dieser Schritt ist optional und lässt sich auch anders automatisieren (z.B. mit `@for`).
Für die Einfachheit des Beispiels, definiere ich aber im Folgenden für jeden Breakpoint einen eigenen Mixin:

```css
@mixin breakpoint-small {
    @media (max-width: 399px) {
        @content;
    }
}

@mixin breakpoint-medium {
    @media (min-width: 400px) and (max-width: 699px) {
        @content;
    }
}

@mixin breakpoint-large {
    @media (min-width: 700px) {
        @content;
    }
}
```

# Schritt 4: Definition eines Mixins für das Responsive Design

Als nächstes definieren wir einen Mixin, der die Breakpoints aus Schritt 3 für das Response Design aufbereitet:

```css
@mixin responsive {
    @include breakpoint-small {
        @content(1);
    }

    @include breakpoint-medium {
        @content(2);
    }

    @include breakpoint-large {
        @content(3);
    }
}
```

# Schritt 5: Verwendung des Responsive Mixins

Das angelegte Responsive Mixin kann nun verwendet werden:

```css
.test-container {
    @include responsive using ($breakpoint) {
        max-width: max-width($breakpoint);
    }
}
```

# Weiter Optimierungen

Das gezeigte Vorgehen lässt sich mit `@for` bzw. `@each` noch weiter optimieren.
Der `responsive` Mixin kann für weitere SASS Funktionen verwendet werden.
