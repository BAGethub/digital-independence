# Linux-Grundlagen

Wir konzentrieren uns auf Ubuntu 24.04 LTS Server.

## 1. Das Linux-Mentalmodell
- Alles ist eine Datei
- Die Dateisystemhierarchie (/etc, /var, /home, /usr, /opt)
- Konfiguration lebt in Textdateien
- Protokolle befinden sich in /var/log
- Services laufen im Hintergrund
- Root ist allmächtig (und gefährlich)

## 2. Die Shell (Bash-Grundlagen)
- pwd, cd, ls, tree
- cat, less, head, tail
- grep
- nano (beginne einfach bevor Vim)
- Tab-Vervollständigung
- Befehlsverlauf
- Pipes |
- Umleitung >, >>
- Absolute vs. relative Pfade
- Versteckte Dateien
- Exit-Codes (echo $?)
- Man-Pages

## 3. Benutzer, Gruppen & Berechtigungen
- Warum du niemals alles als Root ausführen solltest
- Das Sudo-Modell
- Dateieigentümer
- Berechtigungsbits (rwx)
- Numerische Berechtigungen (755, 644)
- Gruppen
- Service-Benutzer (z.B. www-data, docker)
- adduser
- usermod
- passwd
- groups
- chmod
- chown

## 4. Paketverwaltung
- apt
- snap (nur oberflächlich)
- apt update
- apt upgrade
- apt install
- apt remove
- apt autoremove
- apt search
- Repositories verstehen
- Warum zufällige curl | bash Skripte gefährlich sind

## 5. Prozesse & Systemüberwachung
- Was ist ein Prozess
- Vordergrund vs. Hintergrund
- ps
- top / htop
- kill
- systemctl status
- Protokolle mit journalctl

## 6. Cron & geplante Aufgaben
- crontab -e
- Crontab-Syntax
- System vs. Benutzer Cron
- Häufige Fehler
- Protokollierung von Cron-Jobs

## 7. Shell-Scripting-Grundlagen
- Nicht fortgeschritten — aber praktisch.
- Variablen
- $1, $2
- if
- for
- Exit-Codes
- Shebang (#!/bin/bash)
- Scripts ausführbar machen
- Schreiben eines einfachen Backup-Skripts

## 8. Dateisystem & Einhängen (Einführungsstufe)
- Was ist /etc/fstab
- Laufwerke einhängen
- UUID vs. Gerätenamen
- df -h
- du -sh
- Swap-Datei-Konzept

## 9. Updates & Sicherheitsgrundlagen
- Warum Updates wichtig sind
- unattended-upgrades
- Sicherheit vs. Feature-Updates
- CVEs-Konzept
- Überprüfung offener Ports (ss -tulnp)
