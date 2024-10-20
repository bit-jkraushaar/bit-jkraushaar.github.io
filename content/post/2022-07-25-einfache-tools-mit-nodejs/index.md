---
date: "2022-07-25T08:00:00Z"
title: Einfache Tools mit Node.js schreiben
description: "In diesem Artikel wird die Vielseitigkeit von Node.js hervorgehoben, nicht nur für Webanwendungen, sondern auch für die Entwicklung kleinerer Tools, die den Projektalltag erleichtern. Besonders für Projekte, in denen Node.js bereits genutzt wird, bietet dieser Leitfaden praktische Schritte zur Einrichtung eines solchen Projekts. Von der Erstellung eines neuen Projekts mit npm init bis hin zur Nutzung von TypeScript für Typsicherheit werden die nötigen Schritte erläutert, einschließlich der Installation wichtiger Abhängigkeiten wie tslint und ts-node. Zusätzlich werden nützliche Tools wie commander für CLI-Implementierungen und typed-rest-client für typisierte REST-Anfragen vorgestellt."
image: header.png
---

Mit Node.js lassen sich nicht nur Webanwendungen schreiben, sondern auch kleine Tools, die den Projektalltag erleichtern.
Dieser Ansatz ist besonders für Projekte geeignet, bei denen Node.js ohnehin schon im Einsatz ist.
Dieser Artikel zeigt, wie ein solches Projekt aufgesetzt werden kann und welche Abhängigkeiten den Start erleichtern.

Im Weiteren wird vorausgesetzt, dass Node.js bereits installiert ist.
Die Beispiele verwenden Node.js 16.16.0.
Außerdem wird für eine verbesserte Typsicherheit TypeScript eingesetzt.

1. Ein neues Projekt anlegen: `npm init`
1. TypeScript hinzufügen: `npm install --save-dev typescript`
1. Types für Node.js: `npm install --save-dev @types/node`
1. Linting: `npm install --save-dev tslint`
1. Initialisieren von TypeScript: `npx tsc --init`
1. Initialisieren von tslint: `npx tslint --init`

Anschließend ist das Projekt theoretisch einsatzbereit.
Um eine TypeScript Datei direkt ausführen zu können, empfiehlt sich aber noch der Einsatz von ts-node: `npm install --save-dev ts-node`.

Dadurch lässt sich ein Script wie folgt ausführen: `npx ts-node src/mein-script.ts`

Weitere nützliche Abhängigkeiten:

* commander: Hilft bei der Implementierung einer CLI für das Skript.
* typed-rest-client: Einfacher typisierter REST Client