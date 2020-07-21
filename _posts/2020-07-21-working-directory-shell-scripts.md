---
layout: post
title: "Arbeitsverzeichnis in Shell-Skripten referenzieren"
date: 2020-07-21 20:30:00 +0200
---

In Shell-Skripten muss von Zeit zu Zeit das aktuelle Arbeitsverzeichnis referenziert werden.
Hierzu gibt es sowohl unter Windows als auch unter Linux zwei kurze Statements.

- Linux: `$(pwd)`
- Windows: `%cd%`

Ein Beispiel für den Einsatz gibt es im herausragenden [docToolchain](https://github.com/docToolchain/docToolchain/tree/master/bin) Projekt.
