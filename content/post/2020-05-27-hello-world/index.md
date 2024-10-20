---
date: "2020-05-27T16:48:36Z"
title: Hallo Welt
description: "Hallo zusammen! Ich bin Jochen Kraushaar, ein leidenschaftlicher Softwareentwickler und Architekt. Dies ist mein erster Blogartikel, und wie könnte er besser beginnen als mit einem klassischen Hallo Welt? In diesem Blog teile ich meine Best Practices und Erfahrungen aus dem Alltag, um mir selbst und vielleicht auch anderen Entwicklern das Leben zu erleichtern. Erfahrt mehr über meine Reise und den Technologie-Stack, der diesen Blog antreibt. Willkommen bei Jochens Cache!"
image: header.webp
---

Das hier ist mein erster Artikel in diesem neuen Blog.
Und wie es sich für einen Blog Rund um Software Entwicklung gehört, beginnt er natürlich mit *Hallo Welt*.

Mein Name ist Jochen Kraushaar.
Ich arbeite als Software Entwickler und Architekt.
Diesen Blog schreibe ich vor allem für mich selbst.
Er dient mir als Gedächtnisstütze, um Best Practices aus meinem Alltag wiederzufinden, ohne erneut eine aufwändige Recherche durchzuführen.
Oder mit anderen Worten: Er dient mir als Cache - Jochens Cache.

# Anforderungen

Als Software Architekt habe ich zu Beginn dieses Projekts Anforderungen definiert:

* Ich möchte Tools einsetzen, die ich auch in meinem normalen Arbeitsalltag verwende.
* Die Seite soll schnell sein und auf unnötiges JavaScript verzichten.
* Hin und wieder möchte ich Codeschnipsel ohne großen Aufwand einbinden können.
* Aus Sicherheitsgründen soll die Seite so statisch wie möglich sein.
* Gleichzeitig will ich mich aber auch nicht mit dem Design herumschlagen, sondern mich auf den Inhalt konzentrieren.
* Der Build der Seite soll automatisiert sein.

# Erster Versuch: Hugo

Bei meinen Recherchen bin ich auf diesen Beitrag gestoßen: [How to create a blog with AsciiDoc](https://opensource.com/article/17/8/asciidoc-web-development)

Die Autorin kombiniert [Hugo][hugo] mit [AsciiDoc][asciidoc] bzw. [Asciidoctor][asciidoctor] um so einen Blog zu generieren, der dann über [GitHub Pages][github-pages] gehostet wird.
Da ich AsciiDoc bereits in meinem Entwickler-Alltag in der [docToolchain](https://doctoolchain.github.io/docToolchain/) einsetze, wäre dieser Ansatz für mich perfekt.
So könnte ich meine Blogbeiträge in einem Editor (in meinem Fall [Atom][atom]) schreiben und durch Hugo daraus eine statische Seite generieren lassen.
Darüber hinaus bietet mir Hugo auch noch verschiedene Themes und andere Features an, die ich bei einer rein statischen Seite selbst erstellen müsste.

Leider musste ich dann feststellen, dass es zwar grundsätzlich eine Integration in Github Pages gibt, diese aber einen separaten Build erfordert.
Etwas ernüchtert recherchierte ich weiter.

# Zweiter Versuch: Jekyll

Beim genaueren Studium der GitHub Pages Dokumentation fand ich schnell heraus, dass dort bevorzugt [Jekyll][jekyll] eingesetzt wird.
Hier wird der Build direkt von GitHub Pages übernommen.
Leider schränkt GitHub Pages die möglichen Jekyll Plugins ein.
In meinem Fall bedeutet das:

* Kein AsciiDoc-Support (stattdessen wird [Markdown][markdown] verwendet)
* Sehr eingeschränkter I18N-Support

Auf der anderen Seite ist mir eine reibungslose Integration in GitHub Pages sehr wichtig.
Zudem ist die Struktur des Projekts und der Templates etwas einfacher zu verstehen.
Meine Wahl fiel daher auf Jekyll als Generator.
Als Editor nutze ich auch in diesem Fall [Atom][atom].

# Mein Technologie-Stack

Mein Technologie-Stack für diesen Blog sieht nun so aus:

* [Ruby][ruby] - als Voraussetzung für Jekyll
* [Markdown][markdown]
* [Jekyll][jekyll]
* [Atom][atom]

So ausgestattet fehlte mir nur noch ein Impressum und eine Datenschutzerklärung.
Und natürlich musste ich die Anwendung noch irgendwie installieren.

# GitHub Pages

[GitHub Pages][github-pages] bietet sich gerade für Blogs im privaten Umfeld an.
Mit einer Größe von 1 GB für die Seite und 100 GB für die Bandbreite sollte GitHub Pages für so ziemlich jeden privaten Blog geeignet sein.
Das alles ist auch noch kostenlos.

Zudem gibt es eine gute [Getting Started](https://help.github.com/en/github/working-with-github-pages/getting-started-with-github-pages) Dokumentation und eine Dokumentation zur [Integration mit Jekyll](https://help.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll).

Ich bin außerdem ein großer Fan der Idee, den Quellcode dieser Seite über GitHub [bereitzustellen](https://github.com/bit-jkraushaar/bit-jkraushaar.github.io).

# Hello World

Damit ist dieses Projekt bereit, das Licht der Welt zu erblicken. In diesem Sinne:

> Hello World

-- *Every Programming Language ever*

[hugo]: https://gohugo.io/
[asciidoc]: https://asciidoc.org/
[asciidoctor]: https://asciidoctor.org/
[github-pages]: https://pages.github.com/
[atom]: https://atom.io/
[ruby]: https://www.ruby-lang.org/
[jekyll]: https://jekyllrb.com/
[markdown]: https://daringfireball.net/projects/markdown/
