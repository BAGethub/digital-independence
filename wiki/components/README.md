# Komponenten

Es gibt viel Open-Source-Software, die niemals allein verwendet wird, sondern als Teil eines größeren Systems. Dieser Abschnitt gibt einen kurzen Überblick über diese Komponenten.

## Datenbanken

### PostgreSQL

PostgreSQL ist ein leistungsstarker quelloffener relationaler Datenbankserver, der Daten in serverseitig verwalteten Dateien speichert und als separater Dienst ausgeführt wird. Er verwendet MVCC für hochparallele Lese- und Schreibvorgänge mit starken ACID-Garantien und WAL-basierter Absturzwiederherstellung. Er skaliert gut über 100+ GB (bis zu Terabytes), unterstützt Partitionierung, parallele Abfragen und Replikationen für Leseskalierbarkeit. PostgreSQL ist funktionsreich und erweiterbar (erweiterte SQL, benutzerdefinierte Typen, Erweiterungen wie PostGIS), wodurch es eine optimale Wahl für komplexe Abfragen, GIS/Zeitreihen und Produktionssysteme ist, die Datenintegrität benötigen. Es hat auch starke Unterstützung für JSON, sodass es als Hybrid zwischen relationalen und dokumentorientierten Datenbanken verwendet werden kann. Es ist operativer aufwendiger als eingebettete Datenbanken, aber gut geeignet für wachsende, geschäftskritische Bereitstellungen.

### MariaDB

MariaDB ist ein relationaler Datenbankserver (ein Fork von MySQL), der als separater Dienst ausgeführt wird und Daten in serverseitig verwalteten Dateien speichert. Er ist für Multi-User-, gleichzeitigen Zugriff mit vollständigem Client-Server-Protokoll ausgelegt, sodass er viele parallele Lese- und Schreibverbindungen besser bewältigt als eine Single-File-Engine. Da er über das Netzwerk läuft (auch lokal über Sockets), gibt es einen bescheidenen Netzwerk-Overhead im Vergleich zu eingebetteten Datenbanken, aber das ermöglicht auch Skalierbarkeit über Maschinen hinweg, Replikation, Clustering und Tools für Backups und Überwachung. MariaDB skaliert gut über 100+ GB hinaus (und in Terabytes), wenn es richtig konfiguriert und bereitgestellt wird, daher ist es eine solide Wahl für wachsende Anwendungen und Produktionsbereitstellungen. Es bietet mehrere Storage-Engines (InnoDB für ACID-Transaktionsworkloads, MyRocks für schreiboptimierte Anwendungsfälle usw.), reichhaltige SQL-Funktionen und reife Leistungsoptimierungs- und Replikationsoptionen, wodurch es sich für OLTP- und viele OLAP-Workloads in kleinen Teams bis zu großen Unternehmen eignet.

### Sqlite

Sqlite ist eine relationale Datenbank, die die Daten in einer einzelnen Datei speichert. Sie ist hochgradig portierbar und einfach einzurichten, aber wenn man mit einem System arbeitet, das viele parallele Schreibprozesse hat, könnte es Probleme geben. In Bezug auf parallele Leseprozesse kann sie sehr schnell sein, weil sie keinen Netzwerk-Overhead hat. Sie ist möglicherweise nicht die ideale Wahl, wenn Sie erwarten, dass Ihre Datenbank auf 100+ GB wächst, aber für kleinere Teams oder leseintensive Anwendungsfälle könnte sie das beste Werkzeug für die Aufgabe sein. Obwohl sie oft nur für Demos oder Entwicklung verwendet wird, ist sie eine außergewöhnlich gut getestete und bewährte Datenbank, die in Milliarden von Geräten weltweit verwendet wird.

### MongoDB

MongoDB ist eine verteilte, dokumentorientierte NoSQL-Datenbank, die Daten als BSON-Dokumente in Sammlungen speichert, anstatt in Zeilen und Tabellen. Sie wird als Server (oder Cluster) ausgeführt und unterstützt reichhaltige, JSON-ähnliche Dokumente mit verschachtelten Feldern und flexiblen Schemas, was die Entwicklung beschleunigt und es einfach macht, Datenmodelle zu entwickeln. MongoDB behandelt hohe Lese- und Schreib-Concurrency durch eine Kombination aus speicherabgebildetem Speicher (oder WiredTiger-Engine), Sharding für horizontale Skalierung und Replica Sets für hohe Verfügbarkeit und Leseskalierbarkeit. Sie eignet sich gut für halbstrukturierte Daten, schnelle Prototypen und Workloads, die horizontale Skalierung benötigen. Allerdings bietet sie historisch schwächere Transaktionsgarantien (Multi-Dokument ACID wurde später hinzugefügt und hat Limits), und sorgfältige Schema- und Index-Gestaltung sind für die Leistung im großen Maßstab wichtig. Verwenden Sie MongoDB, wenn Flexibilität und elastische Skalierung wichtig sind; verwenden Sie eine relationale Datenbank, wenn stark normalisierte Schemas, komplexe Joins oder strikte Transaktionsintegrität primär erforderlich sind.

### Redis

Redis ist ein In-Memory-Key-Value-Store, der Daten auf der Festplatte persistieren kann; er wird als separater Server ausgeführt und stellt ein einfaches Netzwerkprotokoll bereit. Da Daten hauptsächlich im RAM vorhanden sind, liefert Redis extrem niedrige Latenz bei Lese- und Schreibvorgängen und bewältigt sehr hohen Durchsatz und Concurrency. Er unterstützt reichhaltige Datenstrukturen (Strings, Listen, Sets, Hashes, sortierte Sets, Streams) und Primitive für Pub/Sub, Bitmaps und atomare Operationen, wodurch er ideal für Caching, Session Stores, Leaderboards, Echtzeit-Analysen, Queues und vorübergehenden State ist. Persistierungsoptionen (RDB-Snapshots, AOF) tauschen Haltbarkeit gegen Leistung ein; ohne sorgfältige Konfiguration können Sie letzte Schreibvorgänge verlieren. Für Haltbarkeit und größere Datensätze kann Redis mit festplattengestützter Persistenz und Replikation ausgeführt werden, aber horizontale Skalierung erfordert typischerweise Sharding (Redis Cluster) oder externe Tools. Redis glänzt, wenn Sie ultrahelle Zugriffe, In-Memory-Datenstrukturen oder vorübergehenden State benötigen.

### InfluxDB

InfluxDB ist eine speziell entwickelte Zeitreihendatenbank, die für hochvolumige Erfassung und schnelle Zeitbereichsabfragen von zeitgestempelten Daten (Metriken, Ereignisse, Telemetrie) optimiert ist. Sie speichert Daten in zeitgeordneten, komprimierten Blöcken mit indizierten Tags für effiziente Filterung, unterstützt Aufbewahrungsrichtlinien und Downsampling zur Speicherkontrolle und bietet Abfrage-Tools (InfluxQL/Flux) für gefensterte Aggregationen. Sie glänzt bei Überwachung, Observability, IoT und Echtzeit-Analysen; sie ist nicht die richtige Wahl für allgemeine relationale Transaktionen, komplexe Joins oder nicht-Zeitreihen-OLTP-Workloads.

### Neo4j

Neo4j ist eine native Graphdatenbank, die Daten als Knoten und Beziehungen mit Eigenschaften speichert, optimiert für Traversierungen und verbundene Datenabfragen. Sie bietet ACID-Transaktionen, Indizes und die Cypher-Abfragesprache für ausdrucksstarke Graph-Muster. Neo4j glänzt bei tiefen Beziehungsabfragen (Empfehlungen, Betrugserkennung, Wissensgraphen) mit schnellen Traversierungen selbst bei hochgradig verbundenen Daten; sie skaliert gut für Graph-Workloads, ist aber keine universelle relationale oder Zeitreihendatenbank und kann unterschiedliche Modellierung und betriebliche Sorgfalt für sehr große, schreibintensive Cluster erfordern.

## Reverse Proxies / Webserver

### Nginx
Leichter, hochgradig konfigurierbarer HTTP-Reverse-Proxy und Webserver. Unterstützt SSL/TLS, Load Balancing, Caching und umfangreiches Request-Routing über Konfigurationsdateien. Weit verbreitet mit Docker (offizielle Images) und kleiner Ressourcen-Fußabdruck.

### Traefik
Dynamischer, Docker-nativer Proxy mit automatischer Service-Erkennung (Labels), integriertem HTTPS über Let's Encrypt und Dashboard. Ausgezeichnet für schnelle Einrichtung in Docker Compose; unterstützt HTTP, TCP und Middleware-Chains.

### Caddy
Zero-Config HTTPS standardmäßig (automatisches TLS), einfache Konfiguration und ein sauberes, entwicklerfreundliches Dateiformat. Gut für kleine selbst gehostete Stacks, bei denen automatische Zertifikate und Einfachheit wichtig sind.

### Apache HTTP Server
Reifer Webserver mit robusten Reverse-Proxy-Funktionen über mod_proxy/mod_proxy_http. Reiches Modul-Ökosystem und stabiles Verhalten; Konfiguration ähnlich wie Nginx aber mit Apache-Konventionen.

## Dateispeicherung & Backup

### MinIO

S3-kompatible Objektspeicherung für Backups/Medien (minio/minio)

Restic — containerisierter Backup-Client (restic/restic)
Duplicati — browsergbasierter Backup-Client (duplicati/duplicati)

## Authentifizierung

### Keycloak

## Überwachung

### Prometheus

### Grafana

### Watchtower
Auto-Update Watcher für Container

### Loki
Log Aggregation

## Datenbank-Verwaltung
### Adminer
Leichte DB-Admin-UI (adminer)

### phpMyAdmin
MySQL/MariaDB Web-Admin-UI

### pgAdmin
https://www.pgadmin.org/

