---
date: "2026-03-14T13:00:00Z"
title: 'Der 1-Million-Zeilen-Irrtum: Warum Agentic Coding im regulierten Umfeld (noch) am Menschen scheitert'
description: "
Agentic Coding ist DER Trend Anfang 2026. Durch die Fortschritte der Modelle ist es inzwischen möglich, große Mengen Code in
kurzer Zeit zu produzieren. Dabei wird die Code-Qualität stets besser. Werden wir also bald Software-Entwicklung komplett
automatisieren? Vielleicht - oder vielleicht auch nicht. Zumindest im regulierten Umfeld besteht Grund zum Zweifel.
Dieser Artikel erklärt, warum.
"
image: header.png
---

Agentic Coding krempelt die Software-Entwicklungs-Branche um.
Dabei nehme ich vor allem zwei Strömungen wahr:
In meiner eigenen LinkedIn-Blase scheint Agentic Coding bereits im Arbeitsalltag etabliert zu sein.
Frage ich meine Kolleginnen und Kollegen, die ein breites Spektrum verschiedener Branchen und Unternehmen abdecken, sieht die Sache anders aus.
Viele Unternehmen - insbesondere große Konzerne - befinden sich meinem Eindruck nach derzeit noch im Versuchsstadium.

Die Versprechen des Agentic Codings sind groß: Einzelne Agenten können am Tag Millionen Zeilen Code schreiben.
Entwicklerinnen und Entwickler werden dabei zu Teamleitern, die eine ganze Armee von Agenten steuern.
Leider werden diese Versprechungen oft pauschalisiert.
Dabei ist die Software-Entwicklungs-Branche keinesfalls so homogen wie manch einer behauptet.
Es macht einen großen Unterschied, ob Treiber für ein Gerät, eine App oder eine Business-Anwendung entwickelt werden soll.

Im Folgenden werfe ich einen Blick auf ein Teilgebiet unserer Branche, die ihre eigenen Herausforderungen an das Agentic Coding
stellt: Die Software-Entwicklung im regulierten Umfeld.
Doch bevor ich dazu komme, möchte ich zunächst klären, was ich unter Agentic Coding verstehe.

# Was ist Agentic Coding?

Die Welt der generativen KI dreht sich schnell.
Begriffe kommen und gehen über Nacht oder unterlaufen einen Bedeutungswandel.
Zum Zeitpunkt des Erscheinens dieses Artikels verstehe ich unter Agentic Coding folgendes:

Die (teil)automatisierte Entwicklung von Software durch KI-Agenten auf Basis von Large Language Models.
Dabei spielt es für diesen Artikel erst einmal keine Rolle, ob nur ein einzelner Agent oder ein ganzer Agentenschwarm zum Einsatz kommt.
Die Agenten nehmen innerhalb des Agentic Codings verschiedene (Projekt-)Rollen ein, z.B. als Product Owner, Entwickler:in oder Tester:in.

Der Kontext der Agenten besteht dabei meist aus dem Systemprompt (mit der Rollenbeschreibung), einem Set von Regeln, der eigentlichen Code-Basis und einem Auftrag.
Der Auftrag wiederum wird z.B. als Spezifikationen ([Spec-Driven Development](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)) bereitgestellt.

Beliebte Werkzeuge für das Agentic Coding sind meist CLI-Tools wie z.B. [Claude Code](https://www.anthropic.com/claude-code), die häufig noch mit einem Wrapper oder speziellen Skills versehen werden, die den Entwicklungsprozess steuern.

Ein sehr mächtiger Ansatz, der potenziell in kurzer Zeit große Mengen Code schreiben kann, der dann ebenfalls automatisiert getestet und geprüft wird.
Häufig wird dieses Vorgehen heute außerdem durch ein sogenanntes Harness ([Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)) erweitert, um deterministische Qualitätssicherungstools wie z.B. Linter und Formatter zu ergänzen.

# Was sind regulierte Umfelder?

Sehr einfach ausgedrückt ist ein reguliertes Umfeld immer dann gegeben, wenn Entwicklung, Betrieb und Änderungen von Software durch verbindliche externe und interne Vorgaben (z.B. Gesetze, Normen, Leitlinien sowie QMS-/SOP-Regeln) geregelt sind.
Das betrifft insbesondere Domänen, in denen Sicherheit, Wirksamkeit, Datenschutz, Informationssicherheit oder finanzielle Stabilität kritisch sind.
Beispiele für regulierte Umfelder sind z.B. die Pharma-Industrie, Medizintechnik, Luftfahrt oder der Finanzsektor.

Wichtige Kernpunkte regulierter Umfelder sind dabei für gewöhnlich Themen wie [Traceability](https://en.wikipedia.org/wiki/Requirements_traceability), [Safety](https://de.wikipedia.org/wiki/Funktionale_Sicherheit) und [Liability](https://de.wikipedia.org/wiki/Produkthaftung).
Es muss dabei nachvollziehbar sein, wer wann welche Änderung warum vorgenommen hat.
Dokumentation, kritische Pfade, Risiken und Testfälle sind besonders wichtig.
In vielen regulierten Domänen darf Software erst eingesetzt werden, wenn ein formaler Nachweisprozess (z.B. Validierung, Konformitätsbewertung oder Zertifizierung) die Einhaltung der Anforderungen belegt und dokumentierte Freigaben vorliegen.
Die Einhaltung wird zudem regelmäßig im Rahmen von Audits überprüft.

Die Software-Entwicklung unterscheidet sich daher stark von z.B. der Entwicklung einer Webanwendung.
Kurze Release-Zyklen sind möglich, erfordern im regulierten Umfeld aber belastbare, validierte Prozesse und eine klare risikobasierte Steuerung von Änderungen.
Gleichzeitig verschiebt sich der Aufwand der Software-Entwicklung weg vom Coding hin zu Verifizierung, Validierung und Dokumentation.

Im Schadensfall liegt die Verantwortung häufig zunächst bei Organisation bzw. Hersteller; je nach Rechtsrahmen können jedoch auch Betreiber, Integratoren und weitere Parteien der Lieferkette in die Verantwortung einbezogen werden.

# Was bedeutet das für das Agentic Coding?

KI-Agenten können zwar eine Million Zeilen Code am Tag produzieren - aber am Ende muss nachweisbar sichergestellt sein, dass die Ergebnisse fachlich korrekt, risikobeherrscht und regulatorisch konform sind.
Wer schon einmal einen Code Review durchgeführt hat, erkennt schnell, dass das schlicht und ergreifend nicht möglich ist.
Der Mensch wird hier zum Flaschenhals.
Ein menschliches Review und eine formale Freigabe sind in der Praxis häufig erforderlich, insbesondere für risikoreiche Änderungen und kritische Funktionen.
Massiver KI-Output wird damit zum "Denial-of-Service-Angriff" auf die Qualitätssicherung.

Das heißt nicht, dass der Einsatz von KI-Agenten komplett ausgeschlossen ist.
Doch Agenten bleiben in diesem Kontext immer Werkzeuge, die von einem Menschen bedient werden.
Eine vollständige Automatisierung ist in vielen regulierten Kontexten derzeit nur schwer vorstellbar; der Human-in-the-Loop bleibt praktisch unverzichtbar.
Das reduziert die Menge an Code, die generiert werden kann, erheblich.
Insbesondere bei kritischem Code mit hohem Risiko ist daher die vollumfängliche Absicherung durch Tests ein absolutes Muss.
Der Code sollte daher auch unbedingt durch deterministische Prüfsysteme (z.B. statische Analyse, Architekturtests) zusätzlich geprüft werden.

All diese Maßnahmen gelten grundsätzlich auch für von Menschen geschriebenen Code.
Die Herausforderung bei KI-generiertem Code liegt allerdings darin, dass Entwicklerinnen und Entwickler dazu tendieren, ihm zu sehr zu vertrauen.
Es ist außerdem anstrengender, sich in fremden Code hineinzudenken, als diesen selbst zu schreiben.

# Ausblick

Bis auf Weiteres gilt also: Ohne beträchtliche Fortschritte bei der Zuverlässigkeit von Coding Agenten und gleichzeitiger Anpassung der gesetzlichen Rahmenbedingungen bleibt eine vollständige Automatisierung der Software-Entwicklung im regulierten Umfeld undenkbar.
Völlig unabhängig davon stellen sich auch ethische Fragen: Sind wir überhaupt bereit dazu, die Software in unseren medizinischen Geräten vollständig der KI zu überlassen?

Es bleibt spannend.
