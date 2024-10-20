---
date: "2022-08-23T16:55:00Z"
title: Komponenten in Angular unkompliziert mocken
description: "In unserem aktuellen Blog-Artikel widmen wir uns dem Mocken von Abhängigkeiten in Angular-Tests und betrachten dabei bewährte Methoden sowie eine vielversprechende Alternative: ng-mocks. Diese Einführung gibt Ihnen einen Vorgeschmack darauf, wie ng-mocks den Testprozess vereinfachen könnte, ohne dabei zu viel vorwegzunehmen."
image: header.png
---

Beim Schreiben von Tests für Komponenten in Angular empfiehlt es sich Abhängigkeiten zu mocken, um unerwünschte Nebeneffekte zu vermeiden.
Um im Template eingebundene Komponenten zu mocken, gibt es mehrere Möglichkeiten.

Eine Möglichkeit ist die Verwendung des [NO_ERRORS_SCHEMA](https://angular.io/api/core/NO_ERRORS_SCHEMA) bzw. des [CUSTOM_ELEMENTS_SCHEMA](https://angular.io/api/core/CUSTOM_ELEMENTS_SCHEMA).
Allerdings können dadurch tatsächliche Fehler verschleiert werden, weswegen ich bislang eher Dummy Components eingesetzt habe.

Bei den Dummy Components wird innerhalb des Testfalls eine Kopie der angebundenen Komponente angelegt, die allerdings selbst keinerlei Logik und auch kein Template enthält.
Sie verfügt jedoch über dieselbe Schnittstelle wie das Original.
Diese Komponente wird in TestBed zusätzlich zu der zu testenden Komponente deklariert.

Allerdings hat auch dieser Ansatz Nachteile: Es muss sehr viel Boilerplate Code geschrieben werden und bei Änderungen an der Original-Komponente müssen ggf. auch alle Dummy-Versionen angepasst werden.

Heute hat mich ein Kollege auf [ng-mocks](https://ng-mocks.sudo.eu/) aufmerksam gemacht.
Hier ist es möglich Komponenten unkompliziert über einen Funktionsaufruf zu mocken.
Der Mock implementiert dann ebenfalls dieselbe Schnittstelle wie das Original.
Bei Änderungen am Original sind keine weiteren Code-Anpassungen notwendig.

Mehr Informationen dazu hier: [Dokumentation MockComponent](https://ng-mocks.sudo.eu/api/MockComponent)
