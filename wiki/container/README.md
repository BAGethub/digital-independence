# Container

## Was ist ein Container?
Ein Container ist eine leichte, portable und in sich selbst genügende Umgebung, die alles enthält, was zur Ausführung einer Software notwendig ist: Code, Laufzeitumgebung, Systemwerkzeuge, Bibliotheken und Einstellungen. Container isolieren Anwendungen vom Host-System und voneinander, was Bereitstellung und Skalierung vereinfacht.

**Wichtige Punkte:**
- Container sind keine virtuellen Maschinen – sie teilen sich den Kernel des Host-Betriebssystems.
- Beliebte Container-Engines: Docker, Podman, containerd.

## Warum würde man Container verwenden?

- **Einfachheit:** Container verpacken alle Abhängigkeiten und Konfigurationen, daher müssen Sie Software nicht manuell auf Ihrem Host installieren oder konfigurieren. Wenn beispielsweise eine Anwendung eine bestimmte PHP-Version benötigt, können Sie sie in einem Container ausführen, ohne Ihre PHP-Installation des Systems zu beeinträchtigen.
- **Sicherheit:** Container isolieren Anwendungen voneinander und vom Host. Wenn ein Container kompromittiert wird, ist es für einen Angreifer viel schwieriger, auf den Rest des Systems zuzugreifen. Das Betriebssystem in Containern kann auch so stark reduziert werden, dass ein Angreifer möglicherweise nicht einmal einen Texteditor hat, um Dateien anzuzeigen/zu bearbeiten. Weniger Inhalte im Container bedeuten in der Regel weniger Angriffsfläche. (Einschränkung: schwieriger bedeutet nicht unmöglich, aber mehrschichtige Verteidigung schadet nie)
- **Portabilität:** Container funktionieren auf jedem System, das Docker unterstützt, und ermöglichen es, Anwendungen leicht zwischen Servern, Laptops oder Cloud-Anbietern zu verschieben.
- **Konsistenz:** Entwickler und Benutzer führen die gleiche Umgebung aus, was "es funktioniert auf meinem Computer"-Probleme reduziert.
- **Ressourceneffizienz:** Container sind leicht und teilen sich den Kernel des Host-Betriebssystems, daher können Sie viele Container mit weniger Overhead als virtuelle Maschinen ausführen.
- **Einfache Updates & Rollbacks:** Sie können Anwendungen aktualisieren oder zurückrollen, indem Sie einfach die Container-Image-Version ändern.


## Docker
Docker ist die am weitesten verbreitete Container-Engine. Sie ermöglicht es Ihnen, Container einfach zu erstellen, auszuführen und zu verwalten. Es ist auch das, was die meisten Tutorials oder Open-Source-Projekte verwenden, daher werden wir uns in diesem Kurs auch auf Docker konzentrieren.

Wenn Sie sich ein Open-Source-Projekt auf GitHub ansehen, achten Sie einfach auf eine Datei namens "Dockerfile" oder "docker-compose.yaml" und Sie können fast sicher sein, dass Sie es auf Ihrem Server ohne viel Mühe einrichten können.

## Docker Compose
Docker Compose ermöglicht es Ihnen, Multi-Container-Anwendungen mit einer YAML-Datei zu definieren und auszuführen. Dies wird am häufigsten bei Self-Hosting verwendet.

## Andere Orchestrierungssysteme

In diesem Kurs konzentrieren wir uns in erster Linie auf Docker, aber es ist definitiv nicht das einzige Container-Tool da draußen. Jedes Tool hat seine eigenen Stärken und Schwächen. Das beste Tool hängt vom genauen Anwendungsfall ab. Docker ist im Allgemeinen eine gute Wahl für Anfänger, da die meisten OSS-Projekte bereits ein Dockerfile enthalten und es unwahrscheinlich ist, dass ein Self-Hosting-Projekt eine Skalierbarkeit erfordert, die über ein Docker-Compose-Setup hinausgeht.

Hier sind einige Alternativen in Container-Tools:
- Kubernetes: Erweiterte Orchestrierung für großmaßstäbliche Bereitstellungen über mehrere Maschinen ([kubernetes.io](https://kubernetes.io/))
- Podman: Docker-Alternative ([podman.io](https://podman.io/))
- Containerd:

- [Docker-Dokumentation](https://docs.docker.com/)
- [Docker Compose Docs](https://docs.docker.com/compose/)
- [Kubernetes-Grundlagen](https://kubernetes.io/docs/tutorials/kubernetes-basics/)

---

## Spickzettel
- [Docker-Spickzettel](docker.md)
- [Docker Compose Spickzettel](docker-compose.md)
