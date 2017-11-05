# Maven Cheat Sheet

Maven ist ein Build-Werkzeug. Die Konfiguration für ein Projekt ist in einer
`pom.xml`-Datei abgelegt.

Maven kann mehrere Projekte zu einem
[Multi-Modul-Projekt](https://maven.apache.org/guides/mini/guide-multiple-modules.html)
zusammenfassen. Dazu werden Unterhalb eines Projektes Sub-Module angelegt und in
der `pom.xml` referenziert. Jedes Sub-Modul existiert in einem eigenen
Unterverzeichnis und hat eine eigene `pom.xml` die nur für dieses Modul gilt.

Entweder kann man Maven in dem Verzeichnis mit der `pom.xml` für das
Gesamtprojekt starten. Dann baut Maven das gesamte Projekt mit allen Modulen.
Startet man Maven im Verzeichnis eines spezifischen Moduls, dann beziehen sich
die Maven-Kommandos nur auf dieses Modul.

Sollen Maven Module Definitionen, wie z.B. die Java-Version, von anderen Modulen
übernehmen wird Vererbung eingesetzt. Jede `pom.xml` kann dabei nur von genau
einem anderen Modul erben. Bei einem Multi-Modul-Projekt erben häufig alle
Sub-Module von der `pom.xml` welche die Sub-Module referenziert.

#### Verzeichnisse

Ein Maven-Modul hat eine feste Dateistruktur:

* Im Verzeichnis `src/main` sind alle Dateien des Moduls enthalten.

* Das Verzeichnis `src/test` enthält Dateien, die nur für Tests benötigt werden.

Unterhalb dieser Verzeichnisse liegt ebenfalls eine standardisierte
Verzeichnis-Struktur:

* `java` enthält den Java-Code.

* `resources` enthält Ressourcen, wie Property-, Bilder- oder HTML-Dateien.

#### Kommandos

Die wichtigsten Kommandos für Maven sind:

* `mvn package` lädt alle Abhängigkeiten aus dem Internet herunter, kompiliert
  den Code, führt die Tests aus und erzeugt eine JAR-Datei. Das Ergebnis steht
  im Unterverzeichnis `target` des jeweiligen Moduls bereit. `mvn package
  -DskipTests` führt die Tests nicht aus. `mvn package -DdownloadSources=true
  -DdownloadJavadocs=true` lädt den Source Code und das JavaDoc der abhängigen
  Bibliotheken aus dem Internet. Das JavaDoc enthält eine Beschreibung der API.
  Entwicklungsumgebungen können JavaDoc und Source Code der Bibliotheken dem
  Benutzer darstellen.

* `mvn test` kompiliert und testet den Code, erstellt aber keine JAR-Datei.

* `mvn install` fügt `mvn package` noch einen Schritt hinzu, indem es die
  JAR-Dateien in das lokale Repository (`.m2`-Verzeichnis im Heimatverzeichnis
  des Benutzers) kopiert. So können andere Projekte und Module das Modul als
  Abhängigkeit in der `pom.xml` deklarieren.

* `mvn clean` löscht alle Ergebnisse der vorherigen Builds.

* Maven-Kommandos können kombiniert werden. `mvn clean package` kompiliert also
  alles komplett neu, weil die Ergebnisse der alten Builds vor dem Build
  gelöscht werden.

#### Troubleshooting

Wenn `mvn package` nicht funktioniert:

* `mvn clean package` ausprobieren, um alte Build-Ergebnisse vor dem Build zu
  löschen.

* `mvn clean package -DskipTests` nutzen, um die Tests nicht auszuführen.

* `mvn clean package -Dmaven.test.skip=true` führt die Tests nicht aus und
  kompiliert die Tests auch nicht.

#### Spring Boot

<http://start.spring.io/> bietet eine einfache Möglichkeit, um neue
Spring-Boot-Projekte mit passender `pom.xml`-Datei zu erzeugen. Dazu muss der
Nutzer auf der Website nur einige Einstellungen übergeben. Die Website erstellt
dann das passende Projekt mit einer `pom.xml`.

Das Ergebnis des Maven-Builds bei einem Spring-Boot-Projekt ist ein JAR (Java
Archive). In diesem sind alle Bestandteile der Anwendung einschließlich aller
Bibliotheken enthalten. Java unterstützt dieses Dateiformat direkt. Also kann
ein Microservice mit `java -jar target/microservice-order-0.0.1-SNAPSHOT.jar`
gestartet werden.
