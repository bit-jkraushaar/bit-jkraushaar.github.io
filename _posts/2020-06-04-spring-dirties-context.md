---
layout: post
title:  "Neustart des Spring Kontexts in Tests erzwingen"
tags: Java Spring Tests
---

Beim Arbeiten mit Integrationstests im Spring Umfeld kann es passieren, dass ein Neustart des Spring Kontexts erforderlich ist.
Hier schafft die `@DirtiesContext` Annotation Abhilfe.

# Problem

Für eine Anwendung soll es sowohl synchrone als auch asynchrone Integrationstests geben.
Beim Wechsel zwischen diesen Tests erkennt Spring nicht immer zuverlässig, dass sich die Konfiguration geändert hat.
So werden Tests, die von einer synchronen Ausführung ausgehen, plötzlich asynchron ausgeführt.

# Lösung

Mit der [@DirtiesContext Annotation](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/test/annotation/DirtiesContext.html) kann der Neustart des Kontexts erzwungen werden.
Einen sehr guten Überblick über die Funktionsweise der Annotation bietet [Baeldung](https://www.baeldung.com/spring-dirtiescontext).

Für das angesprochene Problem empfiehlt sich der Modus `BEFORE_CLASS`.

```java
@DirtiesContext(classMode = ClassMode.BEFORE_CLASS)
class MyIntegrationTest {
    // ...
}
```
