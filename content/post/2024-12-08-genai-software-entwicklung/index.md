---
date: "2024-12-08T19:30:00Z"
title: 'GenAI in der Softwareentwicklung: Praktische Tipps und Grenzen'
description: "
Generative KI verändert die Softwareentwicklung wie nie zuvor – von der Code-Generierung bis zur Anforderungsanalyse. Entdecke Best Practices, typische Anwendungsfälle und die Grenzen von Modellen wie Github Copilot. Lerne, wie du KI-Tools effizient und sicher einsetzt, um deine Produktivität zu steigern und smarter zu arbeiten.
"
image: header.webp
---

Die Software Entwicklung gehört vermutlich zu den ersten Berusfeldern, die großflächig von der Einführung der großen Sprachmodelle beeinflusst wurde. Zumindest glaube ich das als Softare Entwickler. Vermutlich bin ich da ein wenig voreingenommen.

Tatsächlich arbeite ich selbst seit deutlich über einem Jahr täglich mit Github Copilot und kann mir meinen Beruf ohne dieses praktische Tool kaum noch vorstellen. Github Copilot war für mich dabei lange Zeit nicht mehr als ein "Autocomplete auf Steroide". Zuletzt hat bei mir aber auch der Einsatz der Chat-Funktion deutlich zugenommen.

Das habe ich zum Anlass genommen, mich einmal intensiver mit den Best Practices für den Einsatz von GenAI in der Software Entwicklung auseinanderzusetzen. Ein Buch und einen mehrstündigen Online Kurs später bin ich ernüchtert. Ich erhoffte mir großartige neue Arbeitsansätze und eine Professionalisierung meiner Fähigkeiten - stattdessen lief es im wesentlichen immer auf ein paar Grundsätze hinaus. Diese möchte ich hier mit euch teilen.

# Kontext ist alles

Große Sprachmodelle wurden auf riesigen Datenmengen trainiert. Sie haben mehr Bücher gelesen, als ich es jemals schaffen werde. Und genau da liegt das Problem: Es fällt ihnen zunächst schwer die Information einzuordnen. Je mehr fachlicher Hintergrund bereitgestellt wird, umso besser wird die Antwort.

Es ist unheimlich wichtig den Sprachmodellen so viel Kontext wie möglich über das Umfeld zu geben, in dem die Anfrage spielt. Für technische Fragen sind das zumindest einmal die verwendete Programmiersprache und eingesetzte Frameworks und Bibliotheken. Es hilft aber durchaus, auch den fachlichen Kontext näher zu beschreiben. Je mehr Wissen ihr teilt, umso wahrscheinlicher erhaltet ihr eine passende Antwort.

# Präzise Formulierungen verbessern das Ergebnis

Wie in der Kommunikation mit Kolleginnen und Kollegen hängt auch bei Sprachmodellen die Qualität des Outputs von präzisen Eingaben ab. Verwendet klare Begriffe und definiert eine Domänensprache, um Missverständnisse zu vermeiden. Das ist kein neues Problem - siehe z.B. die Ubiquitous Language aus dem [Domain-Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design).

Dasselbe Problem betrifft leider auch die Sprachmodelle. Da hilft nur eins: Lernt euch präzise Auszudrücken und Missverständnisse schon im Keim zu ersticken. Das Modell wird euch mit besseren Antworten belohnen.

# Die Erwartungshaltung formulieren

Macht dem Sprachmodell klipp und klar, was ihr eigentlich von ihm wollt: Soll es als Trainer agieren und euch ein neues Konzept erklären? Braucht ihr Hilfe beim Brain Storming? Oder sucht ihr eher einen Experten, der euch beim Lösen eines kniffligen Problems behilflich ist? Was auch immer ihr erwartet, teilt es dem Sprachmodell mit!

Ein Sprachmodell, das als Trainer agieren soll, wird euch z.B. wesentlich mehr Kommentare generieren als ein Sprachmodell mit der Rolle eines Experten.

# Das letzte aus dem Sprachmodell rauskitzeln - Spezialistenrollen

Einige der Quellen, die ich zu diesem Thema gelesen habe, empfehlen zusätzlich den Einsatz einer Spezialistenrolle, also z.B.:

> Agiere für den folgenden Gesprächsverlauf als Experte mit langjähriger Erfahrung im Einsatz einer MongoDB Datenbank und tiefem Wissen zu den Interna der Datenbank. [...]

Ob das wirklich hilft? Ich bin mir noch nicht ganz sicher. Eigentlich sehe ich es eher als eine Erweiterung zu dem Zuweisen einer Rolle. Es scheint aber auch nicht zu schaden. Also probiert es aus und macht eure eigenen Erfahrungen!

# Erwartet keine Wunder

Beim Einsatz von Sprachmodellen müsst ihr vor allem auch ihre Grenzen akzeptieren. Erwartet keine Wunder von ihnen. Noch immer haben sie Probleme mit Halluzinationen, insbesondere wenn eure Probleme spezieller werden. Je allgemeiner sie sind, umso wahrscheinlich kann euch ein Sprachmodell helfen.

Seid insbesondere vorsichtig, wenn euch das Sprachmodell Bibliotheken vorschlägt. Kriminielle haben das bereits als einen neuen Angriffsvektor auf euren Arbeitgeber oder Kunden erkannt und stellen zum Teil Bibliotheken bereit, die besonders häufig von Sprachmodellen "erfunden" werden. Prüft also immer genau, was ihr euch da ins Boot holt - das solltet ihr allerdings sowieso immer tun.

Wie gut euch das Sprachmodell helfen kann, hängt auch vom Bekanntheitsgrad eurer eingesetzten Programmiersprache ab. Bei Python wird es euch z.B. sehr viel besser helfen können, als bei R.

Vertraut niemals blind den Worten eines Sprachmodells!

# Verwickelt das Modell in ein Gespräch

Um Fehler in der generierten Antwort zu entdecken, ist es am einfachsten das Sprachmodell in "ein Gespräch zu verwickeln". Stellt Rückfragen zu der Antwort. Lasst euch einzelne Aussagen erklären. Es kann dann gut sein, dass das Sprachmodell seinen Fehler bemerkt und ihn korrigiert.

Mir ist allerdings auch schon aufgefallen, dass sich die Sprachmodelle zum Teil zu falschen Antworten überreden lassen. Dadurch, dass sie auf Hilfsbereitschaft und Freundlichkeit trainiert sind, neigen sie dazu euch zuzustimmen. Es hilft, wenn ihr neutrale offene Fragen stellt - übrigens auch ein gutes Mittel in der Kommunikation mit Menschen.

# Für welche Zwecke eignen sich Sprachmodelle?

In den mir bekannten Quellen wurden Sprachmodelle eigentlich für so ziemlich alles eingesetzt:

- Generierung von Code
- Dokumentation
- Generieren von Tests
- Anforderungsanalyse
- Refinment von Anforderungen
- Fehleranalyse
- Brainstorming von Lösungen
- Konzeption
- Prototyping
- Code Review
- ...

Es scheint als wären den Modellen hier wenig Grenzen gesetzt. Hier hilft es selbst Erfahrungen zu sammeln. Beachtet dabei jedoch immer, dass die Modelle Fehler machen: Je wichtiger Korrektheit ist, umso kritischer prüft das Ergebnis und besprecht es auch mit (menschlichen) Fachexperten.

# Zusammenfassung

Die Kommunikation mit einem Sprachmodell unterscheidet sich gar nicht so sehr von der Kommunikation mit einem Menschen. Vielleicht hilft euch das: Stellt euch vor, ihr müsst einem Junior Entwickler euer aktuelles Problem erklären:

- Gebt möglichst viel Kontext zu dem Problem
- Formuliert präzise und nutzt die richtigen Fachbegriffe
- Macht eure Erwartungshaltung deutlich
- Stellt Rückfragen, wenn euch etwas in der Antwort komisch vorkommt oder unklar ist

Und ganz wichtig: Macht eure eigenen Erfahrungen! Diese Werkzeuge sind noch sehr neu und wir müssen den richtigen Umgang mit ihnen erst lernen.