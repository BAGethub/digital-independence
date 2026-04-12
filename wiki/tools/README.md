# Anleitung zur Einrichtung von Werkzeugen

Vor der zweiten Sitzung muss jeder Teilnehmer folgende Voraussetzungen erfüllen:

- Eine funktionsfähige **Ubuntu Server 24.04 LTS** virtuelle Maschine
- SSH-Zugang zu dieser VM von ihrem Host-System
- Die Möglichkeit, Dateien über SSH zu übertragen
- Grundlegende Netzwerk-Diagnose-Werkzeuge auf Host und VM verfügbar

Wir arbeiten **shell-first**. GUI-Tools sind optional und nicht der primäre Arbeitsablauf.

---

## 1. Einrichtung der Virtuellen Maschine

Wir verwenden eine virtuelle Maschine, um eine echte Remote-Server-Umgebung zu simulieren.
WSL oder native Linux-Installationen werden in diesem Kurs **nicht** verwendet.

### Virtualisierungssoftware

Primäre Empfehlung:

- **[VirtualBox](https://www.virtualbox.org/)**

Alternativen sind akzeptabel, aber die Anweisungen gehen von VirtualBox-Terminologie aus.

## Ubuntu ISO

Herunterladen:

- Ubuntu Server **24.04 LTS** (manuelle Server-Installation)
- Von: https://ubuntu.com/download/server

---

### Windows

1. Installieren Sie VirtualBox.
2. Erstellen Sie eine neue VM:
   - Typ: Linux
   - Version: Ubuntu (64-Bit)
3. Weisen Sie zu:
   - Mindestens 2 GB RAM
   - 20 GB Festplatte (VDI, dynamisch zugeordnet)
4. Hängen Sie die Ubuntu Server ISO an.
5. Netzwerkmodus:
   - Verwenden Sie **NAT** (Standard), es sei denn, Sie wissen, was Bridged Networking ist.
6. Installieren Sie Ubuntu Server normal.
7. Installieren Sie OpenSSH Server während des Setups (oder später mit `apt`).

---

### macOS

1. Installieren Sie VirtualBox.
   - Apple Silicon Benutzer benötigen möglicherweise einen alternativen Hypervisor, der mit ARM kompatibel ist.
2. Erstellen Sie eine neue Ubuntu (64-Bit) VM.
3. Weisen Sie zu:
   - Mindestens 2 GB RAM
   - 20 GB Festplatte
4. Hängen Sie die ISO an.
5. Verwenden Sie NAT-Netzwerk.
6. Installieren Sie Ubuntu Server 24.04.

---

### Linux (Host)

1. Installieren Sie VirtualBox über den Paketmanager Ihrer Distribution.
2. Erstellen Sie eine neue Ubuntu (64-Bit) VM.
3. Weisen Sie zu:
   - Mindestens 2 GB RAM
   - 20 GB Festplatte
4. Hängen Sie die ISO an.
5. Verwenden Sie NAT-Netzwerk.
6. Installieren Sie Ubuntu Server 24.04.

---

## 2. Grundlegende VM-Netzwerke

Nach der Installation:

Innerhalb der VM:

```bash
ip a
````

Identifizieren Sie Ihre IP-Adresse.

Testen Sie die Konnektivität:

```bash
ping 8.8.8.8
ping ubuntu.com
```

Wenn DNS fehlschlägt, aber IP funktioniert, existiert eine Netzwerkverbindung, aber die Namensauflösung funktioniert nicht.

---

## 3. SSH-Zugang

Wir werden SSH als primäre Interaktionsmethode mit Servern verwenden.

Stellen Sie sicher, dass der SSH-Server innerhalb der VM installiert ist:

```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl enable ssh
sudo systemctl start ssh
```

Überprüfen Sie den Status:

```bash
systemctl status ssh
```

---

### Windows

Modernes Windows beinhaltet OpenSSH in PowerShell.

Testen:

```powershell
ssh user@VM_IP
```

Wenn nicht verfügbar, aktivieren Sie OpenSSH Client über Windows Optional Features.

GUI-Alternative (nicht primärer Arbeitsablauf):
* PuTTY

---

### macOS

Öffnen Sie Terminal.

Verbinden Sie sich:
```bash
ssh user@VM_IP
```

macOS beinhaltet OpenSSH standardmäßig.

---

### Linux (Host)

Öffnen Sie ein Terminal.

Verbinden Sie sich:

```bash
ssh user@VM_IP
```

OpenSSH ist normalerweise vorinstalliert.

---

## 4. SSH-Schlüssel Einrichtung (Empfohlen)

Generieren Sie einen Schlüssel auf Ihrem Host-System:

```bash
ssh-keygen
```

Kopieren Sie den Schlüssel auf die VM:

```bash
ssh-copy-id user@VM_IP
```

Testen Sie passwortlose Anmeldung:

```bash
ssh user@VM_IP
```

---

## 5. Dateiübertragung über SSH

Wir werden Konfigurationsdateien und Skripte mit Shell-Tools übertragen.

### Verwenden von `scp`

Vom Host zur VM:

```bash
scp file.txt user@VM_IP:/home/user/
```

Von der VM zum Host:

```bash
scp user@VM_IP:/home/user/file.txt .
```

### Verwenden von `rsync` (bevorzugt für Verzeichnisse)

```bash
rsync -av ./localdir user@VM_IP:/home/user/
```

GUI-Tools (optional, nicht primärer Arbeitsablauf):

* WinSCP (Windows)
* Cyberduck (macOS / Windows)

---

## 6. Netzwerk-Diagnose (Host + VM)

Sie müssen Konnektivitätsprobleme beheben können.

### Windows

```powershell
ipconfig
ping VM_IP
tracert VM_IP
nslookup ubuntu.com
```

### macOS

```bash
ifconfig
ping VM_IP
traceroute VM_IP
nslookup ubuntu.com
```

### Linux (Host)

```bash
ip a
ping VM_IP
traceroute VM_IP
nslookup ubuntu.com
```

### Innerhalb der VM

```bash
ip a
ss -tulnp
ping 8.8.8.8
ping ubuntu.com
```

---

## 7. Snapshots

Vor größeren Änderungen:

Erstellen Sie einen VM-Snapshot in VirtualBox.

Dies ermöglicht ein Rollback, wenn Konfigurationsfehler auftreten.

---

## 8. Erwarteter Basis-Status vor Lektion 2

Sie sollten in der Lage sein:
* Ihre Ubuntu Server VM zu starten
* Ihre IP-Adresse zu identifizieren
* Per SSH von Ihrem Host darauf zuzugreifen
* Eine Datei mit `scp` zu übertragen
* SSH läuft über `systemctl` zu überprüfen
* Netzwerkkonnektivität innerhalb der VM zu überprüfen

Wenn diese nicht funktionieren, beheben Sie dies lokal vor der Sitzung. Sie können sich jederzeit an andere Kursteilnehmer im Mattermost-Chat wenden.

Dieser Kurs priorisiert:
* Terminal-Nutzung
* Log-Lesefähigkeit
* Verständnis von Netzwerkgrenzen
* Verständnis von Service-Verhalten

GUI-Tools können erwähnt werden, aber operative Fähigkeiten werden über die Shell aufgebaut.
