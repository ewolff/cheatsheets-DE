# Maven Cheat Sheet

Maven ist ein Build-Werkzeug. Die Konfiguration für ein Projekt ist in einer
`pom.xml`-Datei abgelegt. <http://start.spring.io/> bietet eine einfach
Möglichkeit, um neue Spring-Boot-Projekte mit passender `pom.xml`-Datei zu
erzeugen. Dazu muss der Nutzer auf der Website nur einige Einstellungen
übergeben. Die Website erstellt dann das passende Projekt mit einer `pom.xml`.

Maven kann mehrere Projekte zu einem
[Mulit-Modul-Projekt](https://maven.apache.org/guides/mini/guide-multiple-modules.html)
zusammenfassen. Dann sind die Definitionen, die für alle Module gelten sollen,
in einer `pom.xml`hinterlegt. Diese `pom.xml` referenziert alle Module.

Die `pom.xml` ist in einem Verzeichnis abgespeichert. Die Module sind in
Unterverzeichnisse abgespeichert.  Sie haben eine eigene `pom.xml` mit den
spezifischen Informationen für das jeweilige Modul.

Entweder kann man Maven in dem Verzeichnis mit der `pom.xml` für das
Gesamtprojekt starten. Dann baut Maven das gesamte Projekt mit allen Modulen.
Startet man Maven in dem Verzeichnis für ein spezifisches Modul, dann beziehen
sich die Maven-Kommandos auf ein Modul.

#### Verzeichnisse

Ein Maven-Modul hat eine feste Dateistruktur:

* Im Verzeichnis `main` sind alle Dateien des Moduls enthalten.

* Das Verzeichnis `test` enthält Dateien, die nur für Tests benötigt werden.

Unterhalb dieser Verzeichnisse liegt ebenfalls eine standardisierte
Verzeichnis-Struktur:

* `java` enthält den Java-Code.

* `resources` enthält Ressourcen, die in die Anwendung übernommen werden.

#### Kommandos

Die wichtigsten Kommandos für Maven sind:

* `mvn package` lädt alle Abhängigkeiten aus dem Internet herunter, kompiliert
  den Code, führt die Tests aus und erzeugt eine ausführbare JAR-Dateien. Das
  Ergebnis steht im Unterverzeichnis `target` des jeweiligen Moduls bereit.
  `mvn package -DskipTests` führt die Tests nicht aus. `mvn package
  -DdownloadSources=true -DdownloadJavadocs=true` lädt den Source Code und das
  JavaDoc der abhängigen Bibliotheken aus dem Internet. Das JavaDoc enthält eine
  Beschreibung der API. Entwicklungsumgebungen können JavaDoc und Source Code
  der Bibliotheken dem Benutzern darstellen.

* `mvn test` kompiliert und testet den Code, erstellt aber kein JAR.

* `mvn install` fügt `mvn package` noch einen Schritt hinzu, indem es die
  JAR-Dateien in das lokale Repository im `.m2`-Verzeichnis im Heimatverzeichnis
  des Benutzers kopiert. So können andere Projekte und Module das Modul als
  Abhängigkeiten im `pom.xml` deklarieren. Für die Beispiele ist das aber nicht
  notwendig, sodass `mvn package` ausreichend ist.

* `mvn clean` löscht alle Ergebnisse der vorherigen Builds. Maven-Kommandos
  können kombiniert werden. `mvn clean package` kompiliert also alles komplett
  neu, weil die Ergebnisse der alten Builds vor dem Build gelöscht werden.

Das Ergebnis des Maven-Builds ist ein JAR (Java Archive). In dem JAR sind alle
Bestandteile der Anwendung einschließlich aller Bibliotheken enthalten. Java
unterstützt dieses Dateiformat direkt. Also kann ein Microservice mit `java -jar
target/microservice-order-0.0.1-SNAPSHOT.jar` gestartet werden.

#### Troubleshooting

Wenn `mvn package` nicht funktioniert:

* `mvn clean package` ausprobieren, um alte Build-Ergebnisse vor dem Build zu
  löschen.

* `mvn clean package package -DskipTests` nutzen, um die Tests nicht
  auszuführen.
