---
layout: post
title: "Consumer Driven Contracts - Erfahrungsbericht, Teil 1"
date: 2022-09-03 12:00:00 +0200
---

In den letzten Monaten haben wir bei meinem aktuellen Projekt Consumer Driven Contracts eingeführt.
Dabei habe ich einiges über diese Technik gelernt.
Meine Erkenntnisse schreibe ich in einer zweiteiligen Artikelserie nieder.
Dieser Artikel ist der erste Teil und befasst sich mit der Theorie, ohne auf Vor- und Nachteile einer speziellen Implementierung einzugehen.

# Was sind Consumer Driven Contracts?

[Consumer Driven Contracts](https://de.wikipedia.org/wiki/Consumer_Driven_Contracts) sind eine Testmethode für die Kommunikation zweier Services in einer verteilten Architektur.

Klassischerweise wird bei der Kommunikation zweier Services der Contract (d.h. die Schnittstelle) vom Provider vorgegeben und z.B. über OpenAPI als JSON oder YAML Dokument bereitgestellt.
In Fällen, in denen der Provider eine Schnittstelle seine Consumer nicht kennt, ist dieses Vorgehen unvermeidbar.
Wenn die Consumer jedoch bekannt sind, können auch Consumer Driven Contracts genutzt werden.

Während Provider Contracts im zweiten Szenario ebenfalls sehr populär sind, haben sie in meinen Augen doch einige Nachteile:

* Der Provider hat die Macht über die Schnittstelle und kann sie nach belieben spezifizieren.
Für den Consumer bleibt also häufig nur "Friss oder Stirb".
* Eine vom Provider spezifizierte Schnittstelle neigt dazu, durch das in der Datenbank abgebildete Datenmodell beeinflusst zu werden.
Durch technische Vorgaben der Datenbank weicht dieses Datenmodell jedoch häufig vom tatsächlichen fachlichen Datenmodell ab.
Letztendlich schleicht sich somit das technische Datenmodell aus der Datenbank bis in die Consumer ein.
* Gibt es mehr als einen Consumer für eine Schnittstelle, passieren häufig zwei Dinge: 
Zum einen neigt die Schnittstelle dazu Daten auszuliefern, die nur für einen der Consumer interessant sind, für die anderen aber nicht.
Zum anderen verlieren Provider schnell den Überblick, welcher Teil der Schnittstelle von welchem Consumer genutzt wird.
Dadurch wird es für den Provider später sehr schwierig, Änderungen an der Schnittstelle vorzunehmen.
* Beim Deployment des Providers ist häufig nicht klar, ob alle Consumer bereits mit Änderungen an der Schnittstelle klar kommen.
Dem Provider bleibt daher nichts anderes übrig als Änderungen stets abwärtskompatibel zu veröffentlichen.
Umgekehrt können sich Consumer nicht sicher sein, ob der Provider bereits die gewünschten Änderungen vorgenommen hat.

Bei diesen Nachteilen setzen Consumer Driven Contracts an.
Und das bringt mich zu meinem Erfahrungsbericht.

# Erkenntnis 1: Die Arbeitsprozesse ändern sich

Vor dem Einsatz von Consumer Driven Contracts sah unser Arbeitsablauf meist so aus:

1. Wir implementieren die Änderung am Provider.
1. Wir ziehen den Client im Consumer nach.

Jetzt ist es genau umgekehrt: Erst implementieren wir das Verhalten im Consumer und arbeiten so lange gegen Dummy-Daten, bis wir mit dem Datenmodell zufrieden sind.
Anschließend implementieren wir die geänderte Schnittstelle im Provider.

Das ist zu Beginn ein Lernprozess, vergleichbar mit der Einführung von Test Driven Development.
Wenn man so will sind Consumer Driven Contracts TDD für den Provider.

# Erkenntnis 2: Die Fachlichkeit gewinnt an der Schnittstelle an Bedeutung

Da die Schnittstelle jetzt vom Consumer definiert wird, definiert der Consumer die Schnittstelle nach seinen Bedürfnissen.
Diese sind (insbesondere wenn der Consumer ein Frontend ist) meist stark durch die Fachlichkeit beeinflusst.
Die Schnittstelle orientiert sich daher stärker an der Fachlichkeit als bisher, was gleichzeitig die Schnittstelle vom technischen Datenmodell der Datenbank im Provider entkoppelt.

Gibt es mehrere Consumer an einer Schnittstelle, dann wird der Provider nur dann denselben Endpoint verwenden, wenn die Consumer auch dieselbe Fachlichkeit erwarten (abgesehen von kleinen Abweichungen in den erwarteten Daten).
Gibt es größere Abweichungen in der Fachlichkeit, ist es für den Provider einfacher, eine zweite Schnittstelle bereitzustellen.
Im Extremfall gibt es für jeden Use Case eine andere Schnittstelle.
Auf lange Sicht fällt es dem Provider dadurch leichter, die Schnittstellen bei Bedarf anzupassen.

# Erkenntnis 3: Die Unsicherheit beim Deployment verschwindet

Durch Consumer Driven Contracts sind wir in der Lage schon vor dem Deployment eines Consumers oder Providers sagen zu können, ob die Schnittstellen der beiden Services zusammen passen.
Das funktioniert natürlich nur, wenn es keine zirkulären Abhängigkeiten zwischen den Services gibt.

Mit der von uns eingesetzten Software Pact (siehe Teil 2 der Artikelserie) können wir für jede Stage sagen, ob die Kommunikation nach dem Deployment weiterhin sichergestellt ist oder nicht.

Gleichzeitig wird dadurch transparenter in welcher Reihenfolge Services deployed werden müssen.

# Erkenntnis 4: Bei der Intergration muss umgedacht werden

Wird ein Consumer um eine Anforderung erweitert, kann die entsprechende Änderung meist erst veröffentlicht werden, wenn der Provider seine Schnittstelle angepasst hat.
Um die Schnittstelle testen zu können, braucht der Provider aber erst einmal den neuen Contract des Consumers.

In der Praxis heißt das, dass der Contract meist schon bereitgestellt werden muss, bevor der Feature Branch im Consumer gemerged wird.
Das heißt auch, dass Contracts nicht nur in der Version des Services vorliegen, sondern auch in Unterversionen für verschiedene Feature Branches.

Beim Deployment über eine CI/CD Umgebung ist außerdem darauf zu achten, dass der Contract zuvor mindestens einmal durch den Provider geprüft werden muss.
Ansonsten ist der Contract nicht verifiziert und es kann nicht deployed werden (in unserem Fall unterbrechen wir das Deployment und fordern eine manuelle Eingabe an, ob fortgesetzt werden soll - für Notfälle).

# Erkenntnis 5: CDC sind nur so gut wie ihre Tests

Wie auch bei anderen Tests gilt: Die getestete Implementierung ist immer nur so gut wie ihre Tests.

Typischerweise prüft ein Consumer Driven Contract auf die folgenden Fakten:

* Pfad
* Content Type
* Status Code der Antwort
* Body der Antwort (meist nur Korrektheit der Datentypen)

Darüber hinaus können Contracts auch die Inhalte der Antwort z.B. über Regular Expressions oder die Authentifzierung prüfen.
Für uns hat sich gezeigt, dass wir meist mit zwei Tests pro Endpoint gut fahren:

1. Ein Test für den Positivfall, der prüft ob unter dem Pfad die erwartete Antwort zurückkommt.
Dabei ist der Inhalt der Antwort weniger relevant als das Vorhandensein der Properties und ihrer Datentypen.
In Ausnahmefällen prüfen wir aber auch den Inhalt auf Korrektheit, z.B. wenn eine ID einem bestimmten Muster folgen muss.
1. Ein Test für den Negativfall, der uns zeigt, wie sich die Schnittstelle verhält, wenn die angefragten Daten nicht vorhanden sind (404 bzw. leeres Ergebnis).

Authentifizierung oder andere komplexere Szenarien testen wir eher über andere Tests ab.
Für unsere Zwecke war diese Strategie bisher erfolgreich.

# Erkenntnis 6: Die zusätzliche Komplexität ist nicht zu unterschätzen

Consumer Driven Contracts bringen zusätzliche Komplexität mit sich.
Insbesondere durch die Änderungen in den Arbeitsprozessen und bei der Integration können am Anfang Probleme entstehen.
Daher empfiehlt es sich einfach anzufangen und die Unterstützung der Contracts sukzessive auszubauen und zu verbessern.

Vorsicht ist vorallem dann geboten, wenn die Contract Tests immer wieder aus nicht nachvollziehbaren Gründen fehlschlagen oder instabil sind.
Wie bei anderen Tests auch, neigen Entwickler in dem Fall schnell dazu die fehlgeschlagenen Tests zu ignorieren und damit tatsächliche Fehler zu übersehen.

# Erkenntnis 7: Code Generierung und API Dokumentation

Consumer Driven Contracts sind kein Ersatz für Code Generierung von Schnittstellen oder API Dokumentation.
Hier sind andere Technologien wie z.B. OpenAPI deutlich besser aufgestellt.
Allerdings spricht auch nichts dagegen, sich aus der fertigen Implementierung im Provider die API Dokumentation generieren zu lassen.

Allein für das Problem der Code-Generierung haben wir bisher noch keine Lösung gefunden.
Da der Consumer Code vor dem Provider Code geschrieben werden muss, kann der Client nicht aus der Server-Schnittstelle erzeugt werden.
Möglich wäre theoretisch die Erzeugung der Schnittstelle aus dem Test.
Allerdings fehlt hier bei dem von uns eingesetzten Produkt die entsprechende Tool-Unterstützung.

# Fazit und Ausblick

Ich hoffe der Artikel hat einen guten Überblick über meine Erkentnisse bei der Einführung von Consumer Driven Contracts gegeben.
Alles in allem bin ich - trotz der Herausforderungen - sehr zufrieden mit dem Einsatz von CDC und würde es jederzeit wieder tun.

Im zweiten Teil dieser Artikelserie werde ich mich den Erkenntnissen aus dem Einsatz von [Pact](https://pact.io/) widmen.