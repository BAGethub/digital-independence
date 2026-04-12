# Abschlussprojekt

Das Abschlussprojekt umfasst die Dokumentation für das einrichten und den Umstieg auf eine Open Source Software. Die Dokumentation soll so sein, dass sie Veröffentlicht werden kann, also keine IPs, Passwörter oder Schlüssel enthalten.

Für ein Serverprojekt sollten diese Themen behandelt werden:

## Anwendungsfall
- Was möchte ich überhaupt?

## Softwareauswahl
- Beschreibung
- Wartungsstatus
- Gemeinschaft
- Link zum Quellcode
- Link zur Dokumentation
- Lizenz

## Server
- Welches Betriebssystem?
- Welche Hardware (Anforderungen)
- Servertyp (lokal/VPS/...)

## Server-Absicherung & Sicherheit
- Welche Ports sind wirklich nach außen offen, und welche kannst du schließen?
- Hast du einen SSH-Key eingerichtet und den Root-Login deaktiviert?
- Läuft eine Firewall (z. B. ufw) und ist Fail2ban aktiv?
- Wie gehst du mit Passwörtern und API-Keys um. Liegen sie im Klartext irgendwo?

## Sicherungsplan
- Wie werden Sicherungen erstellt?
- Automatisierung?
- Wie wird wiederhergestellt?

## Überwachung
- Wie merkst du, dass dein Dienst ausgefallen ist?
- Schaust du regelmäßig in die Logs und weißt du, wo sie liegen?
- Gibt es einen Alarm oder eine Benachrichtigung, wenn etwas schiefläuft?

## Aktualisierungsstrategie
- Wie hältst du das Betriebssystem und deine Anwendung aktuell?
- Testest du Updates irgendwo, bevor du sie auf dem produktiven Server einspielst?
- Was ist dein Plan, wenn ein Update etwas kaputt macht?

