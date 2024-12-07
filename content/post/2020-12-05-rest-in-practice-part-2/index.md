---
date: "2020-12-05T14:00:00Z"
title: 'REST in der Praxis - Teil 2: Datenaufbereitung'
description: 'Im zweiten Teil meiner Artikelreihe "REST in der Praxis" widme ich mich der essentiellen Thematik der Datenaufbereitung für REST Schnittstellen. Ich zeige die Vielfalt der Lösungsansätze auf, von Datenbank Views bis hin zum manuellen Mapping, und beleuchte deren spezifische Vor- und Nachteile. Dieser Artikel soll euch dabei helfen, die passende Datenaufbereitungsstrategie für eure Projekte zu finden und die Effektivität eurer REST Schnittstellen entscheidend zu verbessern.'
image: header.png
---

In der Artikelreihe "REST in der Praxis" stelle ich verschiedene Themen aus dem Umfeld von REST Schnittstellen vor.
Teil 1 der Reihe habe ich dem [Maturity Model]({% post_url 2020-08-02-rest-in-pratice-part-1 %}) gewidmet.
Dieses Mal beschäftige ich mich mit der Datenaufbereitung für REST Schnittstellen.

Bei der Datenaufbereitung wird das interne Datenmodell - im Folgenden bezeichne ich es auch als Rohdatenmodell - durch Mapping für den Anwendungsfall der Schnittstelle aufbereitet.
Es handelt sich also um eine Sicht bzw. View auf das interne Datenmodell.
Die Aufbereitung entkoppelt dadurch den Konsument der Schnittstelle vom Rohdatenmodell und kann die Menge der zu übertragenden Daten reduzieren.
Gleichzeitig bedeutet das zusätzliche Mapping aber auch ein Mehr an Komplexität für die Anwendung.
Je nach gewählter Lösung kann sich zudem der Speicherverbrauch erhöhen und die Antwortzeit verlängern.

Im Folgenden möchte ich verschiedene Lösungsstrategien betrachten, die mir in letzter Zeit begegnet sind.
Die Liste hat dabei keinen Anspruch auf Vollständigkeit.

Ich gehe für die Lösungen von folgendem Technologie-Stack aus:

* Die Anwendung basiert auf Java.
* Die Kommunikation zwischen Anwendung und Datenbank erfolgt über einen Objekt-Relationalen-Mapper (ORM).
* Als ORM nehme ich Hibernate an, wobei andere ORM genauso denkbar sind.
Einige der Lösungen nutzen allerdings proprietäre Hibernate Features.
* Entsprechend handelt es sich bei dem Datenmodell um ein relationales Datenmodell.
* Die Daten werden in der Schnittstelle durch Jackson nach JSON konvertiert.
Auch hier sind andere Bibliotheken denkbar, aber einige Lösungen verwenden proprietäre Jackson Features.

Die vorgestellten Lösungen können auch für andere Technologie-Stacks funktionieren.

# Lösung 0: Keine Datenaufbereitung

Da es in diesem Artikel um die Datenaufbereitung geht, handelt es sich hierbei streng genommen nicht um eine Lösung.
Dennoch möchte ich die Möglichkeit, auf Datenaufbereitung zu verzichten, nicht außeracht lassen.
Tatsächlich kann es Szenarien geben, in denen der Verzicht sinnvoll ist:

Als Beispiel sei eine Anwendung mit nur wenigen kleinen Tabellen und ohne große Beziehungen zwischen den Tabellen genannt.
In diesem Fall würde Datenaufbereitung ein unnötiges Mehr an Komplexität und Aufwand bedeuten, das vermutlich nicht zu rechtfertigen ist.

Auch bei komplexeren Anwendungen kann die Datenaufbereitung unnötig sein, weil der Konsument z.B. vom selben Team entwickelt wird.
Eine enge Kopplung zwischen Konsumenten und Datenquellen ist zwar im Allgemeinen nicht zu empfehlen, kann aber unter Berücksichtigung des Mehraufwands durch Einführung einer Datenaufbereitung akzeptabel sein.

Wie immer gilt: Die Entscheidung eine Datenaufbereitung einzuführen sollte bewusst getroffen und gut dokumentiert werden.

# Lösung 1: Datenbank Views

Eine der naheliegendsten Lösungen ist der Einsatz von Datenbank Views für die Datenaufbereitung.
Schließlich ist das die Idee von Views: Sie sollen eine andere Sicht auf die Rohdaten anbieten.
Für die Anwendung ist in diesem Fall transparent, ob es sich bei den abgefragten Daten um ein physikalisches Datenmodell oder nur eine Sicht handelt.

Beim Einsatz von JPA muss allerdings folgendes beachtet werden:

* In den allermeisten Fällen können Views nur gelesen werden.
Entsprechend vorsichtig muss mit den Entitäten umgegangen werden.
* Gibt es eine Entität auf die Rohdaten und eine Entität auf die View, so handelt es sich dennoch aus Sicht des OR-Mappers um zwei verschiedene Entitäten.
D.h. eine Änderung an den Rohdaten bekommt die Entität auf die View nicht zwangsweise mit.
Insbesondere beim Einsatz von Caching kann es zu inkonsistenten Daten kommen.
* Soll z.B. eine bestimmte Filterlogik zwischen den Entitäten geteilt werden, kann es notwendig werden, diese doppelt zu entwickeln.

Am besten funktioniert diese Lösung daher, wenn es nur selten schreibende Änderungen gibt oder ganz auf JPA verzichtet wird.

# Lösung 2: Vererbung der Entitäten

Eine weitere Lösungsmöglichkeit besteht im Einsatz von Vererbung bei den JPA Entitäten.
Die Idee besteht darin, eine Mindestmenge an gemeinsamen Daten (z.B. den Primärschlüssel) als abstrakte Oberklasse zu wählen und die verschiedenen Sichtweisen dann als Spezialisierungen zu implementieren.

Allerdings bringt diese Lösung alle Probleme der Vererbung mit sich.
Nicht umsonst gilt: [Composition over Inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance).
Die Vererbung wird hier zudem durch die Technik begründet und nicht durch die Fachlichkeit, was die Verständlichkeit des Datenmodells erschweren kann.

Bei komplexeren Datenmodellen stößt diese Lösung daher sehr schnell an ihre Grenzen.
Es besteht die Gefahr ein schwer wartbares Datenmodell zu entwerfen.

# Lösung 3: Query Projection

Bei der Query Projection wird nicht die komplette JPA Entität ausgelesen, sondern eine Untermenge der verfügbaren Spalten.
Auch ist es möglich Daten für die Projection zu aggregieren oder über Joins Daten aus anderen Tabellen dazu zu laden.
Hibernate bietet Projection als eigenes [Feature](https://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/Hibernate_User_Guide.html#criteria-projection) an.

Wenn man so will, handelt es sich bei der Projection um eine durch den OR-Mapper umgesetzte View auf die Rohdaten.
Dem OR-Mapper ist dabei bewusst, auf welchen Entitäten die Projection beruht.
Er kann etwaige Änderungen an den Daten im Cache nachziehen.
Die zurückgegebenen Objekte können POJOs sein und direkt an den JSON Mapper übergeben werden.

Am besten funktioniert die Projection, wenn es sich nur um eine Untermenge der ursprünglichen Entität handelt.
Vorsicht ist bei Beziehungen zu anderen Entitäten geboten:
Da die Projection selbst keine Entity ist, können z.B. 1:n-Beziehungen auch nicht Lazy aufgelöst werden.
Stattdessen müssen die Daten Eager über Joins geladen werden.
Bei 1:n-Beziehung wird hierbei das kartesische Produkt aus den Tabellen gebildet:
Hat eine Entität Auto vier Entitäten Räder, dann gibt es im Ergebnis vier Reihen:
Auto mit Reifen 1, Auto mit Reifen 2, usw...

Das wird insbesondere dann zum Problem, wenn auf die Autos ein Paging angewendet werden muss.
Wird das Paging Datenbankseitig implementiert, dann findet es auf Basis der Ergebnismenge statt.
Da die Ergebnismenge aber für jedes Auto vier Reihen aufweist, läuft das Paging ins leere.

Alternativ kann das Paging zwar im Speicher stattfinden, je nach Menge der zu erwartenden Daten ist das jedoch keine Option.

# Lösung 4: Entity Graphen

[Entity Graphen](https://www.baeldung.com/jpa-entity-graph) ermöglichen es auf Basis von Anwendungsfällen zu entscheiden, wann Beziehungen zwischen Entitäten Lazy oder Eager aufgelöst werden sollen.
Sie sind seit JPA 2.1 Teil des Standards.

Ein Beispiel:
Ein Blogartikel hat mehrere Kommentare.
Für die Übersichtseite des Blogs sind die Kommentare nicht wichtig.
In diesem Fall können die Kommentare also Lazy geladen werden.
Klickt der Nutzer allerdings auf den Artikel, möchte er alle Kommentare angezeigt bekommen.
Hier kann es Sinn machen, die Kommentare Eager zu laden.

Über den Entity Graphen lässt sich also auswählen, welche Beziehungen zu einer Entität geladen werden.
Dabei gibt es einige Dinge zu beachten:

* Über Entity Graphen lässt sich nur bestimmen, welche Beziehungen aufgelöst werden, nicht aber welche Spalten geladen werden.
Entsprechend werden die ausgelieferten Daten nicht vollständig vom Rohdatenmodell entkoppelt.
* Der JSON Mapper muss so eingestellt werden, dass er Lazy Beziehungen nicht automatisch auflöst.
Ansonsten werden trotzdem alle Daten an der Schnittstelle ausgeliefert.
* Je nach Implementierung konnte ich schon beobachten, dass Eager Beziehungen über Joins aufgelöst wurden.
Dabei kann es wieder zu Problemen mit dem Paging kommen (siehe [Lösung 3](#lösung-3-query-projection)).

# Lösung 5: Filter

Hibernate bietet die Möglichkeit [Filter](https://docs.jboss.org/hibernate/orm/5.4/userguide/html_single/Hibernate_User_Guide.html#pc-filtering) auf Beziehungen zu legen.
Dadurch ist es möglich z.B. nur jene Kommentare zu einem Blogartikel zu laden, die im letzten Jahr verfasst wurden.
Der Filter kann dabei auch parametrisiert werden.

Ähnlich wie bei den Entity Graphen ist es also nicht möglich, die zurückgegebenen Spalten auszuwählen, sondern nur die Beziehungen einzuschränken.
Der Einsatz kann dennoch interessant sein, um z.B. die Menge der geladenen Daten zu reduzieren.
Der Filter wird bei Beziehungen immer dann angewendet, wenn die entsprechenden Entitäten Lazy nachgeladen werden oder es ein Join mit ihrer Tabelle gibt.

Leider ist es nicht möglich, über Filter aus einer 1:n-Beziehung eine Pseudo-1:1-Beziehung zu machen, selbst wenn die abhängigen Entitäten über den Filter distinkt sind.
An der Schnittstelle wird also aus der Beziehung immer ein JSON Array gemacht, auch wenn aus fachlicher Sicht nur ein Datensatz zurückgegeben werden kann.

# Lösung 6: JSON Views

Jackson bietet ein Feature namens [JSON Views](https://www.baeldung.com/jackson-json-view-annotation), über das es möglich ist abhängig vom Anwendungsfall zu bestimmen, welche Daten serialisiert werden sollen.
Damit wird die Datenaufbereitung direkt in den JSON Mapper verlagert.
Ein manuelles Mapping zwischen den Rohdatenmodell und dem aufbereiteten Datenmodell ist nicht notwendig.

Ich habe persönlich noch nicht mit JSON Views gearbeitet und kann daher ihre Vor- und Nachteile nur schwer einschätzen.
Allerdings kann - Stand heute - ein Property nur jeweils einer einzigen JSON View zugeordnet werden.
Sollen Properties in verschiedenen JSON Views verwendet werden, dann muss das über Vererbung von JSON Views geregelt werden - mit all den damit einhergehenden Problemen.

Theoretisch besteht auch die Möglichkeit, die Serialisierung nach JSON selbst zu übernehmen.
Allerdings bedeutet das zusätzlichen Mehraufwand und meiner Erfahrung nach können dabei schnell Fehler passieren, insbesondere wenn das Datenmodell im Nachhinein verändert wird.

# Lösung 7: Manuelles Mapping

Das manuelle Mapping zwischen dem Rohdatenmodell aus der Datenbank und dem ausgelieferten Datenmodell liefert den größten Grad an Flexibilität.
Daten können beliebig aggregiert, projiziert oder transformiert werden.

Allerdings wird dadurch für gewöhnlich der Speicherverbrauch verdoppelt:
Für jedes Objekt im Rohdatenmodell muss es auch ein Objekt im Zieldatenmodell geben (abgesehen von eventuellen Aggregationen).
Werden über eine Schnittstelle sehr große Datenmengen ausgeliefert, kann der Speicherverbrauch durchaus problematisch sein.

Paging ist hingegen (zumindest bei einem 1:1 Mapping) kein Problem.
Auch die Auswirkungen auf die Laufzeit sind in der allermeisten mir bekannten Fällen weniger dramatisch als angenommen.
Bei speziellen Anforderungen an die Antwortzeiten ist dennoch Vorsicht geboten.

Gleichzeitig erhöht sich durch ein manuelles Mapping natürlich der Wartungsaufwand und die Komplexität.

# Fazit

Wie immer gibt es kein "Silver Bullet" für die Datenaufbereitung.
Der gewählte Lösungsansatz ist von den Anforderungen und Rahmenbedingungen abhängig.
Ich möchte auch noch einmal betonen, dass die vorgestellte Liste keinen Anspruch auf Vollständigkeit hat.
Es mag weitere Lösungen geben, die ich hier nicht betrachtet habe.
So ist es z.B. in manchen Datenbanksystemen möglich, direkt JSON auszuliefern.
Dennoch hoffe ich, dass diese Liste der Lösungsmöglichkeiten dem ein oder anderen helfen kann.
