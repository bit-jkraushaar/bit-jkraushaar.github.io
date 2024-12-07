---
date: "2020-06-01T00:00:00Z"
title: Oracle Sequences in Spring Data JDBC 2.0
description: "In diesem Artikel zeige ich dir, wie du Oracle Sequences in Spring Data JDBC 2.0 integrieren kannst. Obwohl diese Version von Haus aus keine Sequences unterstützt, erkläre ich einen praktischen Ansatz, um dies mithilfe von EntityCallbacks zu umgehen. So kannst du effizient ID-Generierung in Oracle-Datenbanken mit Spring Data JDBC umsetzen."
image: header.png
---

Ich beschreibe eine Möglichkeit Oracle Sequences in Spring Data JDBC zu integrieren.
Zum Einsatz kommt dabei Spring Data JDBC 2.0.
Von Haus aus unterstützt diese Version keine Sequences.

# Problem

Spring Data JDBC ermöglicht die Anbindung einer Datenbank ohne Umwege über JPA.
Es setzt außerdem Best Practices aus dem Domain Driven Design um.
Damit ist Spring Data JDBC durchaus eine ernstzunehmende Alternative zu Spring Data JPA.

Leider unterstützt Spring Data JDBC in Version 2.0 nur [Auto-Increment Columns für generierte IDs](https://docs.spring.io/spring-data/jdbc/docs/2.0.0.RELEASE/reference/html/#jdbc.entity-persistence.id-generation).
Oracle Datenbanken setzen hingegen für gewöhnlich Sequences ein.

# Lösung

Spring Data JDBC unterstützt sogenannte [EntityCallbacks](https://docs.spring.io/spring-data/jdbc/docs/2.0.0.RELEASE/reference/html/#jdbc.entity-callbacks). Ich nutze das [BeforeConvertCallback](https://docs.spring.io/spring-data/jdbc/docs/2.0.0.RELEASE/api//org/springframework/data/relational/core/mapping/event/BeforeConvertCallback.html), um vor dem Speichern des Aggregats bei Bedarf eine ID aus der Sequence zu selektieren.

# Beispiel

Zunächst lege ich den `BeforeConvertCallback` an.
Ich markiere ihn als `@Component` und lasse mir das `JdbcTemplate` injizieren.
Dadurch wird der Lebenszyklus des Callbacks von Spring Data JDBC verwaltet.

`BeforeConvertCallback` wird zusätzlich über ein Generic typisiert.
In meinem Fall ist es die Beispielklasse `DemoAggregate`.

```java
import org.springframework.data.relational.core.mapping.event.BeforeConvertCallback;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Component;

@Component
public class SelectSequenceBeforeConvertCallback implements BeforeConvertCallback<DemoAggregate> {

    private final JdbcTemplate jdbcTemplate;

    public SelectSequenceBeforeConvertCallback(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

}
```

Das Interface `BeforeConvertCallback` erfordert die Implementierung der Methode `onBeforeConvert(T)`.
Die Methode wird beim Anlegen, Aktualisieren und Löschen eines Aggregats aufgerufen.
Ich prüfe daher zunächst, ob noch keine ID (der Primärschlüssel) gesetzt ist.
Es handelt sich in diesem Fall um ein neues Aggregat.
Für sie selektiere ich über das `JdbcTemplate` eine neue ID aus der Sequence und weise sie zu.

```java
@Override
public DemoAggregate onBeforeConvert(DemoAggregate demoAggregate) {
    if (demoAggregate.getId() == null) {
        Long id = jdbcTemplate.query("SELECT DEMO_SEQUENCE.NEXTVAL FROM DUAL", resultSet -> {
            if (resultSet.next()) {
                return resultSet.getLong(1);
            } else {
                throw new SQLException("Fehler beim Abrufen der Sequence");
            }
        });
        demoAggregate.setId(id);
    }
    return demoAggregate;
}
```

# Erweiterungsmöglichkeiten

Im beschriebenen Fall behandelt der skizzierte `SelectSequenceBeforeConvertCallback` nur ein einziges Aggregate.
Wenn der Callback für mehrere Aggregate verwendet werden soll, muss für jedes Aggregat ein eigener `BeforeConvertCallback` definiert werden, der die entsprechende Sequence abfragt.
Hier hilft es, die gemeinsam genutzte Funktionalität in eine abstrakte Oberklasse auszulagern und in konkreten Unterklassen die notwendige Konfiguration vorzunehmen.
