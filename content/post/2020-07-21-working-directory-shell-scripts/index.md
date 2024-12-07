---
date: "2020-07-21T20:30:00Z"
title: Arbeitsverzeichnis in Shell-Skripten referenzieren
description: "In meinem heutigen Blogartikel tauchen wir in die Welt der Shell-Skripte ein und erkunden, wie man das aktuelle Arbeitsverzeichnis effektiv referenziert. Egal ob Sie unter Linux oder Windows arbeiten, ich zeige Ihnen die einfachen, aber mächtigen Befehle, die Ihr Skripting-Leben erleichtern werden. Begleiten Sie mich auf dieser technischen Reise, während wir uns Beispiele aus dem bemerkenswerten docToolchain Projekt anschauen. Es ist Zeit, Ihre Shell-Skripte auf das nächste Level zu heben!"
image: header.png
---

In Shell-Skripten muss von Zeit zu Zeit das aktuelle Arbeitsverzeichnis referenziert werden.
Hierzu gibt es sowohl unter Windows als auch unter Linux zwei kurze Statements.

- Linux: `$(pwd)`
- Windows: `%cd%`

Ein Beispiel für den Einsatz gibt es im herausragenden [docToolchain](https://github.com/docToolchain/docToolchain/tree/master/bin) Projekt.
