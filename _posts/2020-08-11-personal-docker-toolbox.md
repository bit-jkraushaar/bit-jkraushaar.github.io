---
layout: post
title: "Persönliche Docker Toolbox erstellen"
date: 2020-08-11 17:30:00 +0200
---

Mit Hilfe von Docker lässt sich einfach eine persönliche Toolbox erstellen, die es erlaubt typische Linux Tools auch unter Windows zu verwenden.
In meinem Artikel zeige ich, wie das `Dockerfile` auszusehen hat und wie der Aufruf über Batch Skripte einfach von der Hand geht.

# Dockerfile

Das Docker Image basiert dabei auf der sehr beliebten Linux Toolsammlung [BusyBox](https://www.busybox.net/), die es auch als [Docker Image](https://hub.docker.com/_/busybox) gibt.
Auf dem BusyBox Docker Image baut das [Alpine Image](https://hub.docker.com/_/alpine) auf, welches unter anderem den Package Manager `apk` ergänzt.
Das Alpine Image wiederum stellt die Grundlage meines Images dar.
Über `apk` können weitere Abhängigkeiten installiert werden, in meinem Beispiel `curl` und `jq`.

*Dockerfile*
```
FROM alpine:3.12
RUN apk add --no-cache curl jq
```

# Build Skript

Um das Bauen unter Windows etwas zu erleichtern (und weil ich mir nicht alle Docker Parameter merken möchte), kann ein Batch Skript verwendet werden.
Folgendes Skript baut das Dockerfile und gibt noch Hinweise zur Verwendung aus.

*build.bat*
```
docker build -t my-toolbox:latest .

echo "Copy my-toolbox.bat to PATH, then run toolbox by entering my-toolbox"
```

# Run Skript

Zum Ausführen der Toolbox verwende ich wiederum ein Batch Skript.
Dieses Skript lässt sich z.B. zur PATH Variable hinzufügen oder in ein entsprechendes Verzeichnis kopieren.

*my-toolbox.bat*
```
docker run --rm -it -v %cd%:/workdir -w /workdir --network host my-toolbox:latest
```

Das Skript:

* führt das gebaute Docker Image in einer interaktiven Shell aus,
* mountet das aktuelle Verzeichnis als `/workdir` in den Container,
* setzt das Arbeitsverzeichnis des Containers auf `/workdir`,
* konfiguriert `host` [Networking](https://docs.docker.com/network/host/) und
* entfernt den Container wieder nach verlassen.

# Ausführen der Toolbox

Ausführen lässt sich die Toolbox dann in der Windows Kommandozeile über das Kommando `my-toolbox`.
