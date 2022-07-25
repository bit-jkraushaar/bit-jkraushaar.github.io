---
layout: post
title: "Einfache Tools mit Node.js schreiben"
date: 2022-07-25 08:00:00 +0200
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