---
date: "2024-12-26T19:00:00Z"
title: 'Was ich beim Advent of Code über ChatGPT gelernt habe'
description: "
"
image: header.webp
---

Seit nunmehr 10 Jahren findet der [Advent of Code](https://adventofcode.com/) (AoC) statt.
Ich habe die diesjährige Ausgabe zum Anlass genommen, um den Einsatz von ChatGPT als Pair-Programmer im Praxiseinsatz zu testen.

# Warum der Advent of Code?

Der AoC findet jedes Jahr statt und umfasst 25 Aufgaben mit zunehmender Komplexität.
Jede Aufgabe besteht aus zwei Teilen, wobei der zweite Teil auf dem ersten aufbaut und meist schwieriger ist als der erste.
Die Aufgaben fassen typische algorithmische Probleme (z.B. Kombinationsprobleme, Graphentheorie und Compilerbau) auf und kleiden sie in eine Geschichte.
Neben der Beschreibung umfassen die Aufgaben auch ein Beispiel, dass die im Text formulierten Regeln verdeutlicht.

Dieser Aufbau eignet sich daher hervorragend für den Test von GenAI Modellen zur Problemlösung:

- Die Aufgabe ist als Textaufgabe gekleidet und erfordert daher eine Transferleistung, die häufig auch im Praxiseinsatz zu beobachten ist.
- Die beigefügten Beispiele dienen als Few-Shot Beispiele für die Anfrage an das Modell und können gleichzeitig zum Testen verwendet werden.
- Die zunehmende Komplexität der Aufgaben erlaubt es, die Leistungsfähigkeit der Modelle besser zu beurteilen.
- Am Ende werden zwei Ergebnisse (für Teil 1 und 2) benötigt, die erlauben die korrekte Funktionsweise des produzierten Codes zu testen.

# Wie bin ich vorgegangen?

Ich habe im Vorfeld ein [Custom GPT](https://chatgpt.com/g/g-673499bad4d88190a1e3aee7a8bbea58-advent-of-code-guide) erstellt, der darauf spezialisiert ist, bei AoC Aufgaben zu unterstützen.
Dieser Assistent umfasst auch über die Möglichkeit ChatGPTs Code Interpreter und Canvas zu verwenden.
Als Sprache für die AoC Aufgaben nutzte ich Python, um von ChatGPTs Fähigkeiten optimal zu profitieren.

Während des AoC habe ich verschiedene Strategien ausprobiert.
Begonnen habe ich dabei mit dem Versuch, ChatGPT die Aufgaben komplett eigenständig lösen zu lassen.
Für die ersten Aufgaben hat das auch sehr gut funktioniert.
Spätere Aufgaben erforderten mehr manuelles Eingreifen meinerseits.
Zum Teil wechselte ich dabei auch auf das o1 Modell von OpenAI.
In einigen wenigen Fällen musste ich komplett manuell implementieren.

Alle Ergebnisse meiner Versuche stehen außerdem in [Google Colab](https://colab.research.google.com/drive/15GWJHp5R8dDY3XGDXEM_-u23zX4MkefA?usp=sharing) bereit.

# Wie lief der Advent of Code für ChatGPT?

# Was habe ich daraus gelernt?