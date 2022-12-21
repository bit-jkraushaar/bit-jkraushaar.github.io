---
layout: post
title: "Consumer Driven Contracts - Erfahrungsbericht, Teil 2"
date: 2022-12-21 16:30:00 +0100
---

In den letzten Monaten haben wir bei meinem aktuellen Projekt Consumer Driven Contracts eingeführt.
Dabei habe ich einiges über diese Technik gelernt.
Meine Erkenntnisse schreibe ich in einer zweiteiligen Artikelserie nieder.
Dieser Artikel ist der zweite Teil und befasst sich mit Pact als Implementierung von Consumer-Driven Contracts.

# Was ist Pact?

[Pact](https://pact.io) ist ein Open Source Tool zum Testen von Schnittstellen-Contracts.
Die Contracts (hier Pacts genannt) werden über Tests im Consumer definiert und anschließend im Provider genutzt, um die Korrektheit der Schnittstelle zu testen.
Pact ist damit eine Implementierung von Consumer-Driven Contracts.

Über das zusätzliche Tool Pact Broker ist es außerdem möglich, die Pacts an einer zentralen Stelle abzulegen.
Darüber hinaus führt der Broker Buch darüber, welche Schnittstellen-Versionen zueinander kompatibel sind und welche Version des Contracts in welcher Umgebung installiert ist.
Dadurch kann der Pact Broker bereits vor dem Deployment die Frage beantworten, ob das zu deployende Artefakt den Contract weiterhin erfüllt oder diesen brechen wird.

Der Pact Broker ist neben den Implementierung in verschiedenen Programmiersprachen - in meinen Augen - ein Hauptargument für den Einsatz von Pact für Consumer-Driven Contracts.
Zudem unterstützt Pact sowohl synchrone (z.B. HTTP) als auch asynchrone (z.B. Messages) Schnittstellen.

Auch wenn Pact zu den besten mir bekannten Implementierungen von Consumer-Driven Contracts gehört, ist das Tool leider nicht komplett problemfrei.
Im folgenden versuche ich daher einige der Best Practices und Stolerpsteine darzustellen.

Ich gehe dabei davon aus, dass Pact für eine Java-Anwendung über JUnit genutzt wird.
Implementierungen anderer Programmiersprachen von Pact mögen auch andere Probleme aufweisen, über die ich an dieser Stelle nichts sagen kann.

# Erkenntnis 1: Namenskonventionen sind wichtig

Ein Pact referenziert eine Reihe von Entitäten, die ausschließlich über Strings definiert werden:

* Consumer
* Provider
* States
* Actions

Consumer und Provider sind selbsterklärend und werden im Kontext von Pact häufig als Pacticipants (sic!) zusammengefasst.
Actions beschreiben eine Aktion, die an der Schnittstelle ausgeführt wird, z.B. das Laden aller Verträge.
States beschreiben einen Zustand, zu dem sich der Provider bei der Anfrage befindet, z.B. sind im System keine Verträge hinterlegt.
States werden im Provider-Test dafür verwendet, die Rückgabe des Systems zu mocken.

Wichtig ist dabei, dass Pact diese Entitäten nur dann als identisch betrachtet, wenn auch der Name (String) identisch ist.
Es empfiehlt sich daher ein Namenskonvention einzuführen, um mögliche Fehlerquellen zu reduzieren.

Außerdem hat Pact Probleme mit Umlauten.
Es empfiehlt sich daher, die Entitäten Englisch zu bennenen.

# Erkenntnis 2: Der richtige Schnitt ist wichtig

Grundsätzlich stellt es Pact den Entwicklern komplett frei, welche Schnittstellen sie unter welchem Consumer bzw. Provider zusammenfassen wollen.
Auch können die States beliebig definiert und so z.B. mehrfach verwendet werden.

In der Praxis hat sich jedoch der folgende Schnitt bewährt:

* Consumer werden nach Anwendung getrennt.
  D.h. alle Zugriffe von Anwendung A kommen vom Consumer A.
* Die Tests sollten auf Consumer-Seite wiederum nach Client getrennt werden.
  Greift Consumer A auf einen Service B und einen Service C zu, sollt es für den Client zu B und den Client zu C jeweils einen eigenen Test geben, der den Contract definiert.
* Provider werden nach Endpoint (bzw. im Spring-Umfeld nach Controller) getrennt.
  Implementiert Service B z.B. einen Endpoint für Verträge und einen Endpoint für Konten, dann sollte es einen Provider "B Contracts" und "B Accounts" geben.
* Für jeden Provider (d.h. Endpoint bzw. Controller) sollte es im Server einen eigenen Test geben.
* States sollten nach Action unterschieden werden.
  Ansonsten kann es zu unerwünschten Nebeneffekten kommen, wenn zwei Actions denselben State nutzen, aber unterschiedliche Mockings erfordern.
* Nutzt ein Client eine Schnittstelle sowohl synchron als auch asynchron, sollten die Consumer in den Contracts dennoch getrennt werden (z.B. "A sync", "A async").

# Erkenntnis 3: Vorsicht bei den Consumer-Tests

Die Contracts werden über Tests im Consumer definiert.
Dabei gibt es jeodch ein paar Stolpersteine:

Der Pact Broker ist dazu in der Lage identische Contracts zu erkennen, selbst wenn sie unterschiedliche Versionsnummern tragen.
Das hat den Vorteil, dass diese Contracts nicht erneut vom Provider verifiziert werden müssen.
Leider lässt sich dieser Mechanismus sehr einfach aushebeln, indem im Contract veränderbare Daten (z.B. Zufallswerte oder das aktuelle Datum) verwendet werden.
In diesem Fall unterscheidet sich jeder Contract vom vorherigen, selbst wenn sie dieselbe Versionsnummer tragen.
Das kann zum Teil zu verwirrenden Problemen führen, wenn ein zuvor bereits verifizierter Pact plötzlich als nicht mehr verifiziert markiert wird.

Während des Consumer-Tests fährt Pact einen Mockserver hoch, der die Schnittstelle simuliert.
Die Tests werden dann gegen diese Schnittstelle ausgeführt und geben u.a. die im Contract definierten Daten zurück.
Diese Daten werden vom Client meist wieder in ein Datenobjekt konvertiert, dass dann im Test überprüft werden kann.
Die Mindestanforderung an diese Überprüfung sollte sein, dass keines der Felder im Datenobjekt "null" ist.
Nur so kann sichergestellt werden, dass das Datenobjekt zum Contract passt.

Dabei ist der Inhalt der Felder weniger wichtig.
Tatsächlich kann der Inhalt der Felder vom Contract zwar z.B. gegen eine Regular Expression gematcht werden, in den allermeisten Fällen genügt es jedoch, wenn ein Feld einfach nur vorhanden ist.

# Erkenntnis 4: Best Practices im Umgang mit JUnit 5 und Spring Boot

* Am besten werden Pacts im Pact Broker abgelegt.
  Auf Provider-Seite können die Pact automatisch vom Pact Broker abgeholt werden.
  Dafür benötigt der Provider-Test die Zugangsdaten und die URL zum Pact Broker.
  Diese können z.B. in Umgebungsvariablen hinterlegt werden.
  JUnit 5 bietet die Möglichkeit auf die Präsenz von Umgebungsvariablen zu testen und ggf. einen Test nicht auszuführen, wenn die Umgebungsvariablen fehlen.
* In meinem Projekt haben wir uns dafür entschieden, dass Endpoints unter dem Kontext "/api" zur Verfügung gestellt werden.
  Diese Abhängigkeit wollten wir aber nicht in den Contracts hartcodieren.
  Entsprechend müssen wir in den Provider-Tests den Request vor der Ausführung manipulieren, damit der Kontext an die URL angehängt wird (siehe dazu die [Dokumentation](https://docs.pact.io/implementation_guides/jvm/provider/junit5spring#modifying-requests)).
* In JUnit können einzelne Use Cases innerhalb eines Tests über Nested Classes abgebildet werden.
  Dabei ist zu beachten, dass für jede Nested Class der Contract geöffnet, eingelesen und neu geschrieben wird.
  Leider gibt es in der aktuellen Pact Version einen [Bug](https://github.com/pact-foundation/pact-jvm/issues/1617), der beim Öffnen des Contracts dazu führt, dass dieser falsch eingelesen wird.
  Es sollte daher auf Consumer Seite auf Nested Classes verzichten werden.
* Die Java-Implementierung von Pact nutzt mittlerweile Version 4 der Spezifikation.
  Leider ist die DSL für diese Version noch unausgereift.
  Als Workaround kann über ".usingLegacyDsl()" auf die alte DSL zurückgegriffen werden.
  Allerdings stehen dann nicht alle Features der neuen Spezifikation zur Verfügung.

# Erkenntnis 5: Pact funktioniert besser, je stärker es in die CI/CD Pipeline integriert wird

Am meisten Nutzen bietet Pact, wenn der Pact Broker eingesetzt wird.
Dann ist es möglich, Deployments an den Broker zu melden und über ein "can-i-use" Kommando zu prüfen, ob die zu deployende Version alle Contracts erfüllt.

Dazu ist es wichtig Pact möglichst vollständig in die CI/CD Pipeline zu integrieren und am besten schon für Feature Branches eigene Contracts zu erstellen.
Außerdem sollten die Pacts auf Provider-Seite regelmäßig verifiziert werden (z.B. mindestens nächtlich).

Hierzu gibt es auch einen [CI/CD Setup Guide](https://docs.pact.io/pact_nirvana).

# Erkenntnis 6: Es ist nicht alles Gold, was glänzt

Auch wenn ich Pact für eine der besten zur Verfügung stehenden Consumer-Driven Contracts Implementierungen im Java-Umfeld halte, ist das Tool doch nicht ganz fehlerfrei:

* Die Dokumentation ist zum Teil lückenhaft oder veraltet.
* Es gibt eine recht lange Liste an Issues, die nur langsam abgearbeitet werden.
* Durch einzelne Schwachstellen (z.B. die Namenskonventionen), kann es von Zeit zu Zeit aufwändig werden, die Contracts zu pflegen.

Vor der Einführung in einem Projekt ist daher gründlich zu prüfen, ob diese Schwachstellen akzeptabel sind.
Wenn sie es sind, bekommt man mit Pact jedoch ein sehr mächtiges Tool, dass die Wartbarkeit von Service-Architekturen erheblich verbessern kann.

# Fazit

Das war der zweite Teil meiner Artikel-Serie zu Consumer-Driven Contracts.
Auch wenn Pact Schwachstellen hat, ist es dennoch die - meiner Meinung nach - beste Implementierung von Consumer-Driven Contracts im Java-Umfeld.
Ich kann daher den Einsatz empfehlen, wenn innerhalb eines Projekts das Zusammenspiel mehrerer Services sichergestellt werden muss und der zusätzliche Aufwand vertretbar ist.