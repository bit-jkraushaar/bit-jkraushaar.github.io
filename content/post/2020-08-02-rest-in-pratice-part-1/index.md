---
date: "2020-08-02T10:30:00Z"
title: 'REST in der Praxis - Teil 1: Maturity Model'
description: "Tauchen Sie mit mir ein in die Welt der REST-Schnittstellen und entdecken Sie das Maturity Model von Leonard Richardson. In meinem neuesten Blogbeitrag führe ich Sie durch die vier Entwicklungsstufen von REST APIs – von den grundlegenden Anfängen bis hin zur vollständigen Ausreifung. Erfahren Sie, wie jede Stufe die Art und Weise, wie wir mit Webdiensten interagieren, transformiert und wie Sie das Modell nutzen können, um Ihre eigenen Schnittstellen zu bewerten und zu verbessern. Ob Sie ein erfahrener Entwickler sind oder gerade erst in das Thema einsteigen, dieser Artikel bietet wertvolle Einblicke und praktische Beispiele, die Ihr Verständnis für REST-Schnittstellen vertiefen werden."
image: header.png
---

In der Artikelreihe "REST in der Praxis" stelle ich verschiedene Themen aus dem Umfeld von REST Schnittstellen vor.
Ich beginne mit dem Maturity Model von Leonard Richardson, ein Versuch den Reifegrad einer REST Schnittstelle formal zu beschreiben.

Vorgestellt hat Leonard Richardson sein Modell bereits [2008](https://www.crummy.com/writing/speaking/2008-QCon/act3.html).
Es beschreibt vier Stufen auf dem Weg zum "Glory of REST".
Im Folgenden werde ich diese Stufen kurz beschreiben.
Eine wesentlich ausführlichere Zusammenfassung bietet [Martin Fowler](https://martinfowler.com/articles/richardsonMaturityModel.html).

# Level 0: The Swamp of Pox

In diesem Level wird zwar auf HTTP als Protokoll gesetzt, die HTTP Methoden werden aber ohne Semantik verwendet (z.B. erfolgen alle Anfragen als GET oder POST).
Die Kommunikation erfolgt häufig als Remote Procedure Call und es gibt nur einige wenige Endpunkte.
Der aufgerufene Use Case hängt demnach allein vom Inhalt der Nachricht ab und nicht vom Endpunkt oder der HTTP Methode.

Wie der Name erahnen lässt, steht Richardson dieser Stufe sehr kritisch gegenüber.
SOAP ist ein klassisches Beispiel für dieses Level.

# Level 1: Resources

In dieser Stufe werden Ressourcen (bzw. Endpunkte) eingeführt, um die Schnittstelle zu strukturieren.
Je nach Use Case werden unterschiedliche URLs angesprochen.
Die ausgeführte Methode hängt allerdings immer noch vom Body der Nachricht ab und nicht von der HTTP Methode.

Ich habe schon Abwandlungen von dieser Stufe gesehen, z.B. in dem die Endpunkte keine Objekte (Substantive) beschreiben, sondern Aktionen (Verben).
Ein typisches Beispiel hierfür wäre der Aufruf von `/person/findByName?name=Max`.

# Level 2: HTTP Verbs

Ab dieser Stufe werden HTTP Verben (GET, POST, ...) verwendet, um Auszuwählen welche Funktion auf den Ressourcen ausgeführt werden soll.
Fast alle Schnittstellen, mit denen ich in der Vergangenheit zu tun hatte, befinden sich auf diesem Niveau.
Tatsächlich ist diese Stufe derart weit verbreitet, dass fast immer von ihr die Rede ist, wenn eine Schnittstelle als "REST Schnittstelle" bezeichnet wird.

Von Roy T. Fielding wurde das allerdings in der Vergangenheit [zurückgewiesen](https://roy.gbiv.com/untangled/2008/rest-apis-must-be-hypertext-driven).
Um von einer REST Schnittstelle sprechen zu können, muss mindestens Level 3 erfüllt sein (siehe unten).

Persönlich würde ich Level 2 noch einmal unterteilen:
Der Einsatz von HTTP Verben allein genügt nicht, sie müssen auch entsprechend dem Standard umgesetzt sein.
Ein Beispiel:
Ein Aufruf von `POST /person` mit einem JSON Body legt eine neue Person an und liefert den Status Code `200` und die angelegte Person zurück.

Laut [HTTP/1.1 Standard](https://tools.ietf.org/html/rfc2616#section-9.5) gilt allerdings:

> If a resource has been created on the origin server, the response
> SHOULD be 201 (Created) and contain an entity which describes the
> status of the request and refers to the new resource, and a Location
> header (see section 14.30).

*Hierzu noch eine Anmerkung: Tatsächlich ist hier von [SHOULD](https://tools.ietf.org/html/rfc2119) die Rede.
Es kann also tatsächlich Ausnahmen geben, die ein Abweichen vom Standard rechtfertigen.
Dabei handelt es sich aber um eine bewusst getroffene Entscheidung, für die es gute Gründe geben muss.*

Die Korrekte Antwort auf einen Aufruf von `POST /person` wäre demnach der Status Code `201`.
Außerdem muss der `Location` Header einen Link auf die neue Ressource enthalten.

# Level 3: Hypermedia Controls

Diese Stufe fügt der vorangegangenen Stufe noch HATEOAS (Hypertext As The Engine Of Application State) hinzu.
Dadurch wird es einem Client möglich, ausgehend von einem bekannten Einstiegspunkt über Links die gesamte API zu erkunden.
Das bietet den Vorteil, dass der Client die Struktur der API nicht kennen muss.
Auch kann der Server zu einem späteren Zeitpunkt den Aufbau seiner API verändern.
So lange der Client nur die Links verwendet, ist der Aufbau der API für ihn transparent.

Meiner Meinung nach ist der Einsatz von HATEOAS vor allem in den folgenden Fällen relevant:

* Client und Server kennen sich nicht,
* Client und Server werden von unterschiedlichen Teams betreut, oder
* Client und Server besitzen unterschiedliche Release-Zyklen.

In all diesen Fällen ist es sinnvoll Client und Server-Anwendungen weitestgehend von einander zu entkoppeln.
Die durch HATEOAS eingeführte Transparenz kann dabei helfen.

Viele der Anwendungen, mit denen ich zu tun hatte, wurden allerdings rein intern genutzt
oder es handelte sich um zwei Anwendungen, die vom selben Team betreut werden (z.B. Single Page Application und Backend).
In diesen Fällen ist der Einsatz von HATEOAS meiner Meinung nach optional, zumal HATEOAS immer noch nicht von allen Tools gut unterstützt wird.

# Fazit

Das Maturity Model von Richardson hilft dabei, den Stand der eigenen Schnittstelle zu beschreiben.
Während Richardson allerdings davon ausgeht, dass jede Stufe auf dem Weg zum "Glory of REST" besser ist als die vorhergehende, sehe ich die Sache etwas differenzierter:
Die Schnittstelle muss zu den Anforderungen passen.
Es kann gute Gründe dafür geben, eine Schnittstelle zu bauen, die in Level 0, 1 oder 2 einzuordnen ist.
Und so lange diese Entscheidung bewusst getroffen wurde, ist auch nichts dagegen einzuwenden.

Besser werden müssen wir allerdings bei der Benennung solcher Schnittstellen.
Eine Level 1 Schnittstelle als "REST Schnittstelle" zu bezeichnen ist bestenfalls gewagt.
Besonders die in der Realität häufig vorkommenden Level 2 Schnittstellen sollten einen eigenen Namen bekommen, um sie besser von Level 3 Schnittstellen unterscheiden zu können.
Bisher ist mir allerdings noch kein passender Begriff untergekommen.
