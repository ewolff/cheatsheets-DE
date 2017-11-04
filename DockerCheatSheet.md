# Docker und Docker Compose Cheat Sheet

Docker Compose dient zur Koordination von mehreren Docker Containern.
Microservice-Systeme bestehen meistens aus vielen Docker Containern.
Daher ist es sinnvoll, die Container mit Docker Compose zu
starten und zu stoppen.

#### Docker Compose

Docker Compose nutzt die Datei `docker-compose.yml`, um
Informationen zu den Container auszulesen. Die
[Docker Dokumentation](https://docs.docker.com/compose/compose-file/)
erläutert den Aufbau dieser Datei.
Abschnitt~\ref{section-docker-docker-compose} enthält ein Beispiel für
eine Docker-Compose-Datei.

Beim Starten gibt `docker-compose` alle möglichen Kommandos aus. Die
wichtigsten Kommandos für Docker Compose:

* `docker-compose build` erstellt die Docker Images für die Container
  mithilfe der `Dockerfiles`.

* `docker-compose pull` lädt die Docker Images vom Docker Hub
  herunter.

* `docker-compose up -d` startet die Docker Container im Hintergrund.
Ohne `-d` starten die Container im Vordergrund, sodass die Ausgabe aller Docker
Container auf der Konsole ausgegeben werden. Dsa ist nicht besonders
übersichtlich. Die Option `---scale`
kann mehrere Instanzen eines Service starten z.B.
`docker-compose up -d ---scale order=2` startet zwei Instanzen des
Order-Service. Vorgabewert ist eine Instanz.

* `docker-compose down` stoppt die Container und löscht sie. Außerdem
wird das Netzwerk gelöscht und die Docker-Dateisysteme gelöscht.

* `docker-compose stop` stoppt die Container. Netzwerk,
  Dateisysteme und Container werden nicht gelöscht.

#### Docker

`docker` gibt beim Start ohne Parameter alle gültigen Kommandos aus.

Tipp: Tab-Drücken vervollständig Namen und IDs von Containern und
Images.

Hier die wichtigsten Kommandos im Überblick. Als Beispiel dient
der Container `ms_catalog_1`:

#### Status eines Containers

* `docker ps` zeigt alle laufenden Docker Container. `docker ps -a`
zeigt auch die gestoppten Docker Container. Die Container haben wie
die Images eine hexadezimale ID und einen Namen.
`docker ps` gibt alle diese Informationen aus.
Für andere Befehle
können Container mit Name oder hexadezimaler ID identifiziert
werden. Meistens
haben die Container Namen wie `ms_catalog_1`. Dieser Name besteht aus
einem Präfix `ms` für das Projekt, den Namen des Service `catalog` und
der laufenden Nummer `1`.
Oft wird der Name des
Containers mit dem Namen des Images (z.B. `ms_catalog`) verwechselt.

* `docker logs ms_catalog_1` zeigt die bisherigen Ausgaben des
      Containers `ms_catalog_1`. `docker logs -f ms_catalog_1` zeigt
      auch alle weiteren Ausgaben an, die der Container noch ausgibt.

#### Lebenszyklus eines Containers

* `docker run ms_catalog ---name="container_name"` startet ein neuen
Container mit dem Image `ms_catalog`, der den
Namen `container_name` erhält. Der Parameter `---name` ist
optional. Der Container
führt dann das Kommando aus, dass im `CMD`-Eintrag des `Dockerfiles`
hinterlegt ist. Man kann aber mit `docker run <image> <Kommando>`
ein Kommando in einem Container ausführen lassen. `docker run
ewolff/docker-java /bin/ls` führt das Kommando `ls` in einem Container
mit dem Docker Image `ewolff/docker-java` aus. Also zeigt das Kommando
die
Dateien im Wurzelverzeichnis des Containers an. Falls das Image lokal
noch nicht vorhanden ist, wird es automatisch vom Docker Hub im
Internet heruntergeladen. Wenn das Kommando ausgeführt worden ist,
beendet der Container sich.

* `docker exec ms_catalog_1 /bin/ls` führt `/bin/ls` im laufenden Container
  `ms_catalog_1` aus. Mit diesen Befehlen kann man also in einem
  bereits laufenden Container
  Werkzeuge starten. `docker exec -it ms_catalog_1 /bin/sh` startet
  eine Shell und leitet Ein- und Ausgabe auf das aktuelle Terminal
  um. So hat man also eine Shell in dem Docker Container zur
  Verfügung und kann interaktiv mit dem Container arbeiten.

* `docker stop ms_catalog_1` hält den Container an. Es
  schickt zuerst einen SIGTERM, damit der Container sauber
  herunterfahren kann, und dann einen SIGKILL. 

* `docker kill ms_catalog_1` beendet die Ausführung des 
  Containers mit einem SIGKILL. Der Container ist aber immer noch vorhanden.

* `docker rm ms_catalog_1` löscht den Container dauerhaft.

* `docker start ms_catalog_1` startet den übergebenen Container wieder.

* `docker restart ms_catalog_1` startet den übergebenen Container neu.

#### Docker Images

* `docker images` zeigt alle Docker Images an. Die Images haben eine
hexadezimale ID und ein Namen. Für andere Befehle können Images mit
beiden Mechanismen identifiziert werden.

* `docker build –t=<name> build <path>` erzeugt ein Image mit dem Namen
`name`. Das `Dockerfile` muss im Verzeichnis `path` liegen. Wenn keine
Version angegeben wird, bekommt das Image die Version `latest`. Als
Alternative kann auch die Version im Format `-t=<name:version>`
angegeben werden. Abschnitt~\ref{section-docker-dockerfiles}
beschreibt das Format der `Dockerfiles`.

* `docker history <image>` zeigt die Schichten eines Images an. Für jede
Schicht wird die ID, der ausgeführte Befehl und die Größe der Schicht
ausgegeben. Das anzuzeigende Image kann mit dem Namen
identifiziert werden, wenn es nur eine Version des Images mit diesem
Namen gibt. Sonst kann Name und Version über `name:version` angegeben
werden. Natürlich ist es auch möglich, die hexadezimale ID des Images
anzugeben.

* `docker rmi <image>` löscht ein Image. So lange noch
ein Container das Image nutzt, kann es nicht gelöscht
werden.

* `docker push` und `docker pull` legen Docker Images in einer Registry
ab oder laden sie aus einer Registry. Wenn keine andere Registry
konfiguriert ist, wird der öffentliche Docker Hub genutzt.

#### Aufräumen

Um die Docker-Umgebung aufzuräumen, gibt es mehrere Kommandos:

* `docker container prune` löscht alle gestoppten Container.

* `docker image prune` löscht alle Images, die keinen Namen haben.

* `docker network prune` löscht alle unbenutzten Docker-Netzwerke.

* `docker volume prune` löscht alle Docker Volumes, die von keinem
Docker Container genutzt werden.

* `docker system prune -a` löscht alle gestoppten Container, alle
  unbenutzten Netzwerke und alle Images, die nicht von mindestens
  einem Container genutzt werden. Es bleibt also nur übrig, was die
  aktuell laufenden Container benötigen.

#### Troubleshooting

Wenn ein Beispiel nicht funktioniert:

* Laufen alle Container? `docker ps` zeigt die laufenden Container
  an. `docker ps -a` auch die terminierten.

* Logs mit `docker logs` anschauen. Das funktioniert auch für beendete
  Conainer. `Killed` in den Logs bedeutet, dass zu wenig Speicher zur
  Verfügung steht. Unter Windows und macOS findet sich die Einstellung
  dafür in der Docker-Anwendung unter Preferences/ Advanced. Docker
  sollten ca. 4 GB zugewiesen sein.

* Bei komplexeren Problemen mit `docker exec -it ms_catalog_1 /bin/sh`
  einem Shell im Container verbinden.



