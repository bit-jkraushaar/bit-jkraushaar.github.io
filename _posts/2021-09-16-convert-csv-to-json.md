---
layout: post
title: "CSV einfach nach JSON konvertieren"
date: 2021-09-16 16:30:00 +0200
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