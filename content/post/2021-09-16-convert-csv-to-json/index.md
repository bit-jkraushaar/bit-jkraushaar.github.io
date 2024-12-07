---
date: "2021-09-16T16:30:00Z"
title: CSV einfach nach JSON konvertieren
description: "In der dynamischen Welt der Softwareentwicklung ist NPM (Node Package Manager) ein zentrales Werkzeug, das Entwicklern eine umfangreiche Bibliothek an Ressourcen bietet. Einer der großen Vorteile von NPM ist die Möglichkeit, bestimmte Bibliotheken direkt über die Kommandozeile zu nutzen. Dieser Artikel beleuchtet die Anwendung des Tools npx, welches die Ausführung von NPM-Paketen vereinfacht und insbesondere die Konvertierung von CSV in JSON zu einem kinderleichten Prozess macht. Wir werden uns speziell auf die Verwendung der Bibliothek csvtojson konzentrieren und zeigen, wie man damit effizient CSV-Dateien in JSON umwandeln kann, ein Prozess, der in der modernen Datenverarbeitung immer wichtiger wird."
image: header.png
---

NPM ist eine riesige Sammlung an Bibliotheken.
Einige dieser Bibliotheken lassen sich auch über die Kommandozeile als Tools ausführen.
Möglich wird das über das Tool [npx](https://npmjs.com/package/npx).
Damit ist es auch möglich, auf einfachem Weg CSV nach JSON zu konvertieren.

Hierfür nutze ich die Bibliothek [csvtojson](https://npmjs.com/package/csvtojson).
Beispielsweise konvertiert das folgende Kommando CSV-Dateien, die mit dem Strichpunkt als Trenner arbeiten, nach JSON:

```
npx csvtojson --delimiter=; input.csv > output.json
```