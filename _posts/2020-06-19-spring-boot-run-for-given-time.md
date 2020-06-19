---
layout: post
title:  "Spring Boot Server nach gegebener Zeit herunterfahren"
tags: Java Spring
---

Manchmal soll ein Spring Boot Server nach einer gegebenen Zeit oder dem Eintreten einer Bedingung heruntergefahren werden.
Eine Demoanwendung soll beispielsweise zunächst einige Kommandos ausführen und sich danach automatisch beenden.
In diesem Artikel zeige ich einen Weg, wie das zu bewerkstelligen ist.

# CommandLineRunner

Grundlage für die vorgestellte Lösung ist ein `CommandLineRunner`.
Dieser wird beim Starten der Spring Boot Anwendung automatisch gestartet.
Ein Beispiel:

```java
public class MyCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("Hello CommandLineRunner!");
    }
}
```

Die übergebenen Argumente entsprechen den Argumenten aus der `main(String[])` Methode.
Der `CommandLineRunner` kann in der Application-Klasse als Bean initialisiert werden.
Alternativ ist es auch möglich, dass die Application-Klasse selbst das Interface implementiert.
Für die vorgestellte Lösung wähle ich die Initialisierung als Bean:

```java
@SpringBootApplication
public class DemoApplication {
    @Bean
    public CommandLineRunner runner() {
        return new MyCommandLineRunner();
    }


    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

# Automatisches Beenden der Anwendung

Da unsere `CommandLineRunner` Implementierung von Spring verwaltet wird, können wir dort Objekte injizieren lassen.
Um den Lebenszyklus der Anwendung zu beeinflussen, brauchen wir den `ConfigurableApplicationContext`.
Das kann zum Beispiel so aussehen:

```java
public class MyCommandLineRunner implements CommandLineRunner {
    @Autowired
    private ConfigurableApplicationContext ctx;

    @Override
    public void run(String... args) throws Exception {
        System.out.println("Hello CommandLineRunner!");
        Thread.sleep(10000L);
        ctx.close();
    }
}
```

Natürlich ist der Einsatz von `Thread.sleep(long)` alles andere als schön.
Außerdem wird hierdurch der Thread schlafen gelegt.
Er steht also nicht für andere Operationen zur Verfügung.
Für einfache Demo-Anwendungen ist das aber (meist) ausreichend.
