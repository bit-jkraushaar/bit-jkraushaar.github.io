---
date: "2020-05-30T08:19:00Z"
title: Custom Progress Bar für Wizards
description: "Vor kurzem habe ich mich der Herausforderung gestellt, einen Wizard mit einem Fortschrittsbalken komplett ohne Komponentenframework zu erstellen. Dafür habe ich ausschließlich HTML und CSS verwendet. Der Clou: Der Fortschrittsbalken basiert auf dem CSS clip-path Property, mit dem sich komplexe Formen einfach gestalten lassen. In diesem Beitrag zeige ich, wie ich ein Oktagon als Basisform für den Fortschrittsbalken verwendet habe und gebe Einblicke in die Umsetzung und mögliche Erweiterungen."
image: header.png
---

Kürzlich stand ich vor der Herausforderung einen Wizard mit Fortschrittsbalken ohne Komponentenframework zu erstellen.
Für den Fortschrittsbalken verwendete ich daher ausschließlich HTML und CSS.

Die Grundlage für den Fortschrittsbalken ist das CSS `clip-path` Property.
Dieses Property wird für [Basic Shapes](https://developer.mozilla.org/en-US/docs/Web/CSS/basic-shape) von allen modernen Browsern [unterstützt](https://caniuse.com/#feat=mdn-css_properties_clip-path_basic_shape).
Mit der `polygon()` Funktion lassen sich relativ einfach komplexe Formen erstellen.
Glücklicherweise gibt es hierfür bereits einen [Online Editor](https://bennettfeely.com/clippy/), der mir die Gestaltung erleichterte.

# Beispiel

Für mein Beispiel verwende ich ein Oktagon als Grundform.

```css
.step {
  -webkit-clip-path: polygon(30% 0%, 70% 0%, 100% 30%, 100% 70%, 70% 100%, 30% 100%, 0% 70%, 0% 30%);
  clip-path: polygon(30% 0%, 70% 0%, 100% 30%, 100% 70%, 70% 100%, 30% 100%, 0% 70%, 0% 30%);

  background: red;

  width: 100px;
  height: 50px;

  line-height: 50px;
  text-align: center;
}
```

Der Hintergrund kann frei gestaltet werden, ebenso die Höhe und Breite.
Die beiden Properties `line-height` und `text-align` dienen zur Zentrierung der Beschriftung.

Zusätzlich soll bei einem Fortschrittsbalken innerhalb eines Wizards der aktive Schritt markiert werden.
In meinem Beispiel verwende ich für den aktiven Schritt eine andere Hintergrundfarbe:

```css
.step.active {
  background: orange;
}
```

Das dazu gehörende HTML sieht z.B. so aus:

```html
<div style="display: flex">
  <div class="step">
    Schritt 1
  </div>
  <div class="step active">
    Schritt 2
  </div>
  <div class="step">
    Schritt 3
  </div>
</div>
```

Dieses Beispiel steht auch auf JSFiddle bereit: [Beispiel](https://jsfiddle.net/a0jz8xwu/)

# Mögliche Erweiterungen

Über die CSS Selektoren [:first-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:first-child), [:last-child](https://developer.mozilla.org/en-US/docs/Web/CSS/:last-child) und [:not](https://developer.mozilla.org/en-US/docs/Web/CSS/:not) lässt sich die Form des ersten bzw. letzten Schritts im Fortschrittsbalken anpassen.
