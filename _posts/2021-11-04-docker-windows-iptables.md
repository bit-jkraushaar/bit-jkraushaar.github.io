---
layout: post
title: "Docker for Windows: Pakete in einem Bridge Netzwerk umleiten"
date: 2021-11-04 16:00:00 +0100
---

Kürzlich hatte ich das Problem, dass ein Prozess in einem Docker Container Pakete an eine falsche IP-Adresse schickt:
Statt an den Host gingen die Pakete an das Standard Gateway des Bridge Netzwerks.
Leider ließ sich die IP-Adresse im Prozess wegen eines Bugs nicht anpassen.
Unter Windows funktioniert zudem das Host Netzwerk von Docker nicht richtig.
Die einzige Wahl war daher die Umleitung der Pakete an die richtige IP-Adresse.

Möglich ist das durch die Verwendung von `iptables`.
Docker nutzt `iptables` selbst, um den Netzwerk-Traffic des Containers zu isolieren.
Um selbst Änderungen vorzunehmen, muss das Argument `--cap-add=NET_ADMIN` beim Ausführen des Containers hinzugefügt werden.
Anschließend kann folgendes Kommando als `root` ausgeführt werden:

```
iptables -t nat -I OUTPUT 1 -p TCP --destination-port 5432 -j DNAT --to-destination `dig +short host.docker.internal`
```

Dadurch wird folgendes bewirkt:
In der Tabelle `nat` wird in der Chain `OUTPUT` an erster Stelle eine Regel hinzugefügt, die alle Pakete betrifft, die an Port `5432` gerichtet sind.
Diese Regel ändert das Ziel des Pakets (`DNAT`), indem es auf eine andere IP-Adresse umleitet.
Die IP-Adresse wird hier dynamisch mittels `dig` aus dem Hostname `host.docker.internal` ermittelt, der wiederum die IP-Adresse des Hosts enthält.

Als Chain muss `OUTPUT` und nicht etwa `PREROUTING` verwendet werden, weil `PREROUTING` nicht auf Pakete zutrifft, die im Container erzeugt werden.
Die Regel muss zudem an die erste Stelle gesetzt werden, damit sie vor den durch Docker hinzugefügten Regeln ausgeführt wird.
Beispielhaft werden hier alle Pakete abgefangen, die an einen bestimmten Port gehen.
Natürlich sind dafür auch andere Kriterien denkbar.

**Wichtig:** Die IP-Adresse des Hosts kann sich ändern.
Daher wird hier die IP-Adresse aus dem Hostname `host.docker.internal` ausgelesen.
Das bedeutet auch, dass das `iptables` Kommando bei jedem Start des Containers neu ausgeführt werden muss.
Beim Herunterfahren des Containers werden die Regeln zurückgesetzt, sodass alte Regeln beim erneuten Hochfahren nicht extra gelöscht werden müssen.