---
date: "2020-07-12T00:00:00Z"
title: Docker Compose Werkzeugkasten für Entwickler
description: "In meinem heutigen Artikel tauche ich in die Welt von Docker Compose ein und teile meine Sammlung an typischen Konfigurationen, die die Entwicklungsarbeit vereinfachen. Früher bedeutete die Einrichtung von Anwendungen wie Datenbanken stundenlange Arbeit, doch mit Docker Compose reduziert sich das auf einen einfachen Befehl. Ich stelle mein GitHub Repository vor, das als praktischer Werkzeugkasten dient, um die Komplexität des Docker CLI zu überwinden und Entwicklern zu ermöglichen, sich auf das Wesentliche zu konzentrieren: die Entwicklung. Entdecken Sie, wie diese Tools Ihren Workflow revolutionieren können."
image: header.png
---

Docker hat die Arbeit von uns Entwicklern revolutioniert.
Mussten früher Anwendungen wie z.B. eine Datenbank aufwändig installiert werden, geht das heute mit einem einzigen Befehl.
Einer der größten Nachteile bei Docker ist meiner Meinung nach allerdings das vergleichsweise komplexe CLI.
Hier setzt Docker Compose an.
Um die Arbeit noch weiter zu erleichtern, habe ich mit der Sammlung typischer Docker Compose Konfigurationen begonnen.

Diese Sammlung steht in einem eigenen GitHub Repository bereit: [docker-compose-dev-toolkit](https://github.com/bit-jkraushaar/docker-compose-dev-toolkit).
Als Lizenz setze ich dabei auf die [Unlicense](https://choosealicense.com/licenses/unlicense/), die die freie Verwendung der Beispiele erlaubt.

Im Moment gibt es Beispiele für:

* [PostgreSQL](https://www.postgresql.org/)
* [MailHog](https://github.com/mailhog/MailHog)

Mit der Zeit werde ich die Liste der Beispiele noch erweitern.
Der Fokus liegt dabei stets auf der Einfachheit.
Als Entwickler möchte ich mich auf die Implementierung der Anwendung konzentrieren und nicht zu viel Zeit in die Konfiguration der Entwicklungsumgebung stecken.
