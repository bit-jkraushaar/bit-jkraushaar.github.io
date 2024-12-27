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

Die ersten beiden Tagen liefen sehr gut für ChatGPT. Ich konnte hier den Aufgabentext 1:1 von der Webseite kopieren und mit der Aufforderung "Bitte hilf mir dabei Teil 1 des heutigen Rätsels in Python zu lösen" an ChatGPT übergeben.

Am dritten Tag musste ich ChatGPT zum ersten Mal korrigieren, konnte dabei meine Hinweise auf die Fachlichkeit beschränken und musste nicht tiefer in den Code einsteigen.
Bereits ab dem vierten Teil musste ich dann (für den zweiten Teil der Aufgabe) bereits einen Blick in den Code werfen.

Ab dem fünften Tag verwendete ich zudem die Aufforderung, dass ChatGPT den Code Interpreter verwenden und sich selbst korrigieren soll.
Damit war es zunächst in der Lage einfache Fehler selbsständig zu finden und zu lösen.
An späteren Tagen verlief sich ChatGPT jedoch zunehmend in der Autokorrektur und stellte unzählige Versuche den Code zu korrigieren, allerdings ohne eine tatsächliche Lösung herbeizuführen.

Mit zunehmenden Komplexität wurde mein eigenes Wissen um mögliche Lösungsansätze immer wichtiger.
Häufig hatte ChatGPT zwar eine Idee, wie das Problem zu lösen sei, verwendete dabei aber den falschen Ansatz.
Leider war das Modell dabei zum Teil so sehr von den eigenen Ansätzen überzeugt, dass es nicht aus den Fehlern im ersten Teil der Aufgabe lernte - obwohl sich die Lösung im selben Kontextfenster befand.

Mit Tag 9 wurden die Probleme seitens ChatGPT immer größer die Aufgabe ohne größere Hilfestellung meinerseits zu lösen.
Das dies bereits so früh der Fall war, kam etwas unerwartet für mich.
Zwischendrin gab es dann aber immer wieder Tage, an denen ChatGPT bereits im ersten Anlauf die richtige Lösung fand.
Meiner Beobachtung nach geschah das vorallem dann, wenn sich die Aufgaben auf bekannte algorithmische Probleme bezogen, die ohne Modifikationen angewendet werden konnten.

Grundsätzlich tendiert ChatGPT zu einfachen Lösungen, die häufig einen Brute Force Ansatz verfolgen.
Das hilft beim Verständnis des produzierten Codes, ist aber für den Advent of Code ungeeignet.
Fast alle Aufgaben arbeiten mit sehr großen Ergebnisräumen, was speziell angepasste Algorithmen erfordert.
Besonders auffällig war dabei die Vorliebe von ChatGPT für die Breitensuche.
Wann immer ein Problem auf einen Graphen oder einen Baum zurückzuführen war, schlug es zunächst die Breitensuche vor.
Leider gelang es ChatGPT dabei nicht immer diese Breitensuche fehlerfrei zu implementieren, obwohl es sich um einen Standardalgorithmus handelt.

Insbesondere bei Aufgaben die 2D Karten mit Reihen und Spalten verwenden scheint ChatGPT Probleme zu haben.
In einigen Fällen musste ich hier den produzierten Code Schritt für Schritt durchgehen und entwirren.
Dabei fiel mir auch auf, dass es schwierig sein kann dem Code zu folgen, ganz so wie bei der Durchführung von Code Reviews anderer Entwickler.

Ab Tag 13 begann ich zunehmend damit, ChatGPT anzuweisen wo immer möglich 3rd Party Bibliotheken einzusetzen:
Zum einen entspricht das dem tatsächlichen Alltag in der Software-Entwicklung und zum anderen konnte ich so auf gut getestete Bibliotheken zurückgreifen, statt selbst den generierten Code debuggen zu müssen.
An diesem Tag probierte ich auch zum ersten Mal das o1 Modell von ChatGPT aus und war von seiner Leistungsfähigkeit überrascht.
Wo das Standardmodell zuvor noch mehrere Ansätze und massive manuelle Hilfe benötigte, war o1 bereits beim ersten Versuch in der Lage das Problem zu lösen.
Leider musste ich in den folgenden Tagen feststellen, dass auch das o1 Modell an seine Grenzen kam.
Irgendwann schlug dann das Rate Limit zu und ich sah von der weiteren Verwendung ab.

Tag 15 war der erste Tag, an dem mich ChatGPT (selbst das o1 Modell) komplett im Stich lies.
Selbst der Versuch Testfälle zu schreiben und diese als Metrik zu verwenden, damit ChatGPT den generierten Code selbsständig verbessern kann, schlug kolosal fehl:
Statt die zwei fehlschlagenden Tests zu verbessern wurden die anderen erfolgreichen Tests auch noch gebrochen.
Am Ende war ich gezwungen die Lösung komplett selbst zu schreiben.

Ab Tag 16 wechselte ich noch einmal meine Strategie und begann ChatGPT zunehmend kleinere Schritte ausführen zu lassen.
Das half vorallem mir beim Verständnis des generierten Codes.
Andererseits konnte ich dabei auch stärker beeinflussen, welcher Lösungsweg eingeschlagen wurde.

Bei der Aufgabe von Tag 17 half mir ChatGPT auf sehr ungewöhnliche weise: Statt hier selbstständig die Lösung zu entwickeln, half es mir dabei den Input (ein sehr einfaches Programm) zu analysieren.
Dadurch war ich in der Lage eine eigene Lösung zu entwickeln.
Hätte ich das selbstständig tun müssen, wäre ich sehr lange beschäftigt gewesen.

In den nächsten Tagen begann ich außerdem damit, ChatGPT mehr wie einen Junior-Entwickler bzw. eine Junior-Entwicklerin zu behandeln und gemeinsam an einer Lösung zu arbeiten.
Dadurch konnte ich meine Erfahrung und Bauchgefühl besser einbringen.

Ab Tag 20 hatte ChatGPT zunehmend Probleme die Aufgaben zu lösen und ich musste immer mehr Aufgaben übernehmen.
Dennoch war das Modell hilfreich beim Vorschlagen von Optimierungen, wenn ich gezielt danach fragte.
Es stellte sich auch heraus, dass ich manchmal schlicht und ergreifend daran scheiterte, die Probleme des Algorithmus in worte zu fassen.
Manchmal war es daher für mich einfacher das Problem selbst zu lösen, als es umständlich auszuformulieren.

Tag 22 war dann wieder ungewöhnlich einfach zu lösen.
Dabei konnte ich aber ein interessantes Problem von ChatGPT beobachten:
Während die initiale Lösung von ChatGPT richtig war, verstand ich die Aufgabe zunächst falsch und baute daraufhin einen Fehler in den Code ein.
Statt auf dem eigenen (korrekten) Verständnis zu beharren, schwenkte ChatGPT ohne zu zögern auf meine (fehlerhafte) Lösung um.

Tag 24 war schließlich ein komplettes Desaster.
Schon für den ersten Teil brauchte ich unzählige Iterationen.
Selbst als ich eine mögliche Lösung skizzierte machte es noch unzählige Fehler.
Der zweite Teil der Aufgabe war dann schlichtweg unmöglich zu lösen.
Ich musste hier selbst aktiv werden und konnte es selbst nur halb-automatisiert lösen.

Der letzte Tag war dann wieder einfach zu lösen, vermutlich als eine Art Weihnachtsgeschenk.

# Was habe ich daraus gelernt?

- ChatGPT bevorzugt einfache Brute Force Ansätze (z.B. die Breitensuche).
- ChatGPT macht selbst bei Standardalgorithmen wie der Breitensuche immer noch Fehler.
- Auch für generierten Code gilt: Code der nicht geschrieben wird, ist guter Code. Es empfiehlt sich der Einsatz von 3rd Party Bibliotheken.
- Je mehr Code generiert wird, umso schwerer fiel es mir mich in diesen Code hineinzudenken. Tatsächlich beobachtete ich, dass mein Gehirn faul wurde und gar nicht erst versuchte das Problem richtig zu verstehen.
- Löste ich das Problem hingegen Schritt für Schritt gemeinsam mit ChatGPT, trat dieser Effekt nicht auf und der Code war besser verständlich für mich. Die Fehlersuche fiel mir leichter.
- ChatGPT (und andere Sprachmodelle) fehlt es vor allem noch an zwei menschlichen Fähigkeiten: Erfahrung und Bauchgefühl. Gerade bei schwierigen Problemen sind diese beiden Fähigkeiten unheimlich wichtig.
- Am besten funktioniert die Zusammenarbeit, wenn man sie ähnlich wie bei der Zusammenarbeit mit einem sehr unerfahrenen Kollegen oder Kollegin gestaltet: Zunächst lässt man das Problem durch ChatGPT erklären bzw. "in eigenen Worten" wiedergeben. Anschließend führt man ein gemeinsames Brainstorming nach möglichen Lösungsansätzen durch. Nach der Auswahl der Lösungsstrategie prüft man zunächst, ob es Bibliotheken gibt, die diesen Ansatz unterstützen und die Implementierung ggf. erleichtern. Dann implementiert man gemeinsam Schritt für Schritt die Lösung und versieht jeden Schritt mit Debugausgaben oder Tests.
- Kommunikation, d.h. die Verwendung der richtigen Begriffe und die präzise Formulierung der eigenen Wünsche, ist beim Einsatz von Sprachmodellen extrem wichtig. Je ungenauer und schwammiger die Formulierungen, umso schlechter ist meist das Ergebnis.
- ChatGPT tendiert dazu, die Meinung des Nutzers höher zu bewerten als den eigenen Code. Während das in den allermeisten Fällen wünschenswert ist, führt das von Zeit zu Zeit auch dazu, dass der Mensch (in diesem Fall ich) Fehler einbaut, die davor nicht vorhanden waren.

**Zusammengefasst:** ChatGPT kann ein sehr wertvoller Pair-Programmer sein, wenn man das Modell richtig einsetzt und es nicht zu viel Code auf einmal generieren lässt. Beim komplexen Aufgabestellungen ist es weit davon entfernt, selbsständig eine Lösung zu finden, selbst bei fortgeschrittenen Modellen wie o1.

**Was bedeutet das im Hinblick auf das neue o3 Modell?**

Das o3 Modell soll angeblich noch einmal deutlich leistungsfähiger als das o1 Modells ein.
Es kann daher durchaus sein, dass einige der Aufgaben mit dem o3 Modell einfacher zu lösen sind.
Allerdings sind die Kosten für dieses Modell sehr hoch (eine Anfrage kann bis zu mehreren tausend Dollar kosten).
Meiner Beobachtung nach spielt die genaue Formulierung der Probleme beim Lösen der Aufgaben - neben der Leistungsfähigkeit des Modells - eine ausschlaggebende Rolle.
Da die Probleme auch in Zukunft von uns Menschen formuliert werden, gehe ich auch bei o3 davon aus, dass mehrere Iterationen notwendig sind.
Vor dem Hintergrund des immensen Ressourcenverbrauchs halte ich es daher für fragwürdig, ob o3 tatsächlich im praktischen Einsatz innerhalb der Software-Entwicklung eine Verbesserung bringt.