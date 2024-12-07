---
date: "2020-06-04T00:00:00Z"
title: Neustart des Spring Kontexts in Tests erzwingen
description: "Beim Arbeiten mit Integrationstests im Spring-Umfeld kann es vorkommen, dass ein Neustart des Spring-Kontexts erforderlich wird, um zuverlässige Testergebnisse zu gewährleisten. In meinem neuesten Beitrag erkläre ich, wie die @DirtiesContext Annotation dabei hilft, dieses Problem zu lösen, insbesondere beim Wechsel zwischen synchronen und asynchronen Testszenarien."
image: header.png
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
