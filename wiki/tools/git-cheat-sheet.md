# Git - Spickzettel

Git ist ein **Versionskontrollsystem**.

Es ermöglicht dir:

* Änderungen an Dateien nachzuverfolgen
* Zu früheren Versionen zurückzukehren
* Sicher mit Branches zu experimentieren
* Dein Projekt auf einen entfernten Server zu synchronisieren (optional)
* Kommentare zu jeder Änderung hinzuzufügen

Stell dir Git als eine **Zeitmaschine für deine Konfigurationsdateien und deinen Code** vor.

Die Erfahrung zeigt: Du wirst vergessen, was du vor ein paar Monaten an deinen Konfigurationen geändert hast. Die Möglichkeit, zurückzurollen und Kommentare zu lesen, warum etwas geändert wurde, ist ein wertvolles Sicherheitsnetz.

---

## 1. Git auf Ubuntu Server installieren

Auf Ubuntu Server (22.04 LTS oder neuer):

```bash
sudo apt update
sudo apt install git
```

### Was das bedeutet:

* `sudo` → Mit Administratorrechten ausführen
* `apt update` → Paketliste aktualisieren
* `apt install git` → Git aus den Ubuntu-Paketquellen installieren

Installation prüfen:

```bash
git --version
```

---

## 2. Git einrichten (einmalig)

Git speichert, wer Änderungen gemacht hat. Git ist dafür ausgelegt, von mehreren Personen auf verschiedenen Maschinen genutzt zu werden. Daher benötigt es Informationen darüber, wer Änderungen vornimmt.

```bash
git config --global user.name "Dein Name"
git config --global user.email "deine@email.com"
```

Für unseren Anwendungsfall nutzen wir Git nicht in vollem Umfang – daher spielt es meist keine große Rolle, was du hier einträgst. Die Angaben müssen keine echten Daten sein, wenn du nicht mit anderen zusammenarbeitest. Wenn du das Repository jemals öffentlich machen möchtest, beachte, dass diese Informationen als Metadaten sichtbar werden. Für Einzelprojekte empfiehlt es sich, eine Noreply-E-Mail-Adresse und einen Alias als Namen zu verwenden.

Aktuelle Git-Konfiguration anzeigen:

```bash
git config --list
```

---

## 3. Wichtige Begriffe

| Begriff           | Bedeutung                                                                     |
| ----------------- | ----------------------------------------------------------------------------- |
| Repository (Repo) | Ein von Git verfolgter Ordner                                                 |
| Commit            | Ein gespeicherter Schnappschuss deiner Dateien                                |
| Branch            | Eine parallele Version deines Projekts                                        |
| Merge             | Zusammenführen von Branches                                                   |
| Remote            | Eine Kopie deines Repos an einem anderen Ort (z.B. GitHub oder eigener Server) |
| HEAD              | Deine aktuelle Position in der Git-Historie                                   |
| Pull              | Repository von einem Git-Remote-Server auf dein lokales Repository herunterladen |
| Push              | Deine lokalen Commits auf den Git-Remote-Server hochladen                     |
| Fetch             | Aktuellen Stand vom Git-Remote-Server holen, ohne lokale Dateien zu überschreiben |

---

## 4. Ein neues Repository anlegen

In deinen Projektordner wechseln:

```bash
cd mein-projekt
```

Git initialisieren:

```bash
git init
```

Das erstellt ein verstecktes `.git`-Verzeichnis, das die Historie speichert.

Status prüfen:

```bash
git status
```

---

## 5. Den Git-Workflow verstehen

Git hat 3 Hauptbereiche:

1. **Arbeitsverzeichnis** – Deine Dateien
2. **Staging-Bereich** – Dateien, die für den Commit vorbereitet sind
3. **Commit-Historie** – Gespeicherte Schnappschüsse

### Typischer Workflow:

```bash
git status
git add dateiname
git commit -m "Beschreibe was geändert wurde"
```

---

## 6. Ersten Commit erstellen

Alle Dateien zum Staging hinzufügen:

```bash
git add .
```

Commit erstellen:

```bash
git commit -m "Erster Commit"
```

Jetzt hast du deinen ersten Wiederherstellungspunkt.

---

## 7. Historie anzeigen

```bash
git log
```

Kurzversion:

```bash
git log --oneline
```

Jeder Commit hat eine eindeutige ID (Hash).

---

## 8. Zurückrollen (Die wichtigste Fähigkeit)

### Änderungen anzeigen

```bash
git diff
```

---

### Datei auf letzten Commit zurücksetzen

```bash
git restore dateiname
```

---

### Alles auf letzten Commit zurücksetzen (ACHTUNG: löscht Änderungen)

```bash
git reset --hard
```

---

### Zu einem früheren Commit zurückgehen

Commit-ID herausfinden:

```bash
git log --oneline
```

Dann:

```bash
git checkout COMMIT_ID
```

Zurück zur neuesten Version:

```bash
git checkout main
```

---

### Sicherste Methode zum Zurückrollen (für Anfänger empfohlen)

Einen neuen Commit erstellen, der einen früheren rückgängig macht:

```bash
git revert COMMIT_ID
```

Das hält die Historie sauber und sicher.

---

## 9. Branches (Sicheres Experimentieren)

Branches ermöglichen es dir, zu experimentieren, ohne die Hauptkonfiguration zu beschädigen.

---

### Branch erstellen

```bash
git branch test-feature
```

---

### Zu einem Branch wechseln

```bash
git checkout test-feature
```

Oder mit der modernen Methode:

```bash
git switch test-feature
```

---

### Erstellen und wechseln in einem Schritt

```bash
git checkout -b test-feature
```

---

### Branches anzeigen

```bash
git branch
```

---

## 10. Branches zusammenführen

Zurück zu main wechseln:

```bash
git checkout main
```

Branch mergen:

```bash
git merge test-feature
```

Bei keinen Konflikten verbindet Git die Historien.

---

## 11. Mit einem Remote verbinden (Optional)

Ein Remote ist einfach eine weitere Kopie deines Repos an einem anderen Ort.

Remote hinzufügen:

```bash
git remote add origin https://github.com/benutzername/repo.git
```

Remotes anzeigen:

```bash
git remote -v
```

Repo hochladen:

```bash
git push -u origin main
```

Änderungen herunterladen:

```bash
git pull
```

---

## 12. Pull Requests (Zusammenarbeit über GitHub)

Ein **Pull Request** (kurz **PR**, deutsch etwa „Änderungsantrag") ist kein Git-Befehl, sondern
eine Funktion von GitHub, GitLab und ähnlichen Plattformen. Er ist die Bitte:

> „Bitte übernimm die Änderungen von meinem Branch in `main`."

`git merge` (Abschnitt 10) führt Branches **sofort und lokal** zusammen. Ein Pull Request
schiebt genau diesen Merge auf und macht ihn vorher sichtbar und diskutierbar.

```
main                    A───B───C          ← bleibt unverändert
                                 ╲
mein-feature                      D───E    ← hier wird gearbeitet
                                      ↑
                              PR = "D und E nach main übernehmen?"
```

### Warum das nützlich ist

| Funktion | Nutzen |
| --- | --- |
| Diff-Ansicht | Zeilenweise sehen, was sich ändert, **bevor** es übernommen wird |
| Kommentare | Rückfragen direkt an einzelnen Zeilen, Nachbesserung ohne Neuanfang |
| Automatische Tests | CI-Prüfungen laufen automatisch (in diesem Repo z.B. der Link-Checker) |
| Merge-Knopf | Nichts landet in `main`, bevor jemand bewusst zustimmt |
| Historie | Später nachvollziehbar, *warum* etwas so entschieden wurde |

Der wichtigste Punkt: Ein PR ist ein **Vorschlag mit Vorschau**, keine Änderung.
`main` bleibt unangetastet, bis jemand zustimmt.

---

### Der typische Ablauf

**1. Branch anlegen und darauf arbeiten**

```bash
git checkout -b mein-feature
```

**2. Wie gewohnt committen**

```bash
git add .
git commit -m "Beschreibe was geändert wurde"
```

**3. Branch zum Remote hochladen**

```bash
git push -u origin mein-feature
```

Das `-u` merkt sich den Remote-Branch. Danach genügt bei weiteren Commits `git push`.

**4. Pull Request eröffnen**

Nach dem Push zeigt GitHub im Terminal einen Link an, etwa:

```
remote: Create a pull request for 'mein-feature' on GitHub by visiting:
remote:      https://github.com/benutzer/repo/pull/new/mein-feature
```

Diesen Link öffnen, Titel und Beschreibung ausfüllen, absenden. Alternativ findet sich auf
der Repo-Seite ein Knopf „Compare & pull request".

**5. Rückmeldungen einarbeiten**

Kommt eine Rückfrage, arbeitest du einfach auf demselben Branch weiter:

```bash
git add .
git commit -m "Rückmeldung eingearbeitet"
git push
```

Der PR aktualisiert sich **automatisch** — kein neuer PR nötig.

**6. Mergen**

Ist alles in Ordnung, klickt jemand auf GitHub „Merge pull request". Erst jetzt ändert sich
`main`.

**7. Lokal aufräumen**

```bash
git checkout main
git pull
git branch -d mein-feature
```

---

### Entwurfs-PR (Draft)

Ein **Draft Pull Request** signalisiert „noch in Arbeit, bitte noch nicht mergen". Der
Merge-Knopf ist gesperrt, Kommentare und Tests laufen aber schon. Nützlich, um früh
Rückmeldung zu bekommen. Später per Klick auf „Ready for review" umstellen.

---

### Muss man PRs benutzen?

Nein. Wenn du allein an einem Repo arbeitest, reicht der direkte Weg aus Abschnitt 10:

```bash
git checkout main
git merge mein-feature
git push origin main
```

PRs lohnen sich, sobald **mehrere Personen** am selben Repo arbeiten oder automatische
Prüfungen laufen sollen. Für diesen Kurs heißt das: Beiträge zum gemeinsamen Repo bitte
über einen PR, damit alle sehen, was sich ändert.

---

### Fremde Repositories: Fork

Bei einem fremden Repo hast du keine Schreibrechte und kannst keinen Branch pushen. Der Weg
dorthin führt über einen **Fork** – eine eigene Kopie des Repos unter deinem Account:

1. Auf GitHub auf „Fork" klicken
2. Deine Kopie klonen: `git clone https://github.com/DEIN-NAME/repo.git`
3. Branch anlegen, committen, in **deinen** Fork pushen
4. PR von deinem Fork zum Original-Repo eröffnen

So funktionieren praktisch alle Beiträge zu Open-Source-Projekten.

---

## 13. `.gitignore` verwenden (SEHR WICHTIG)

Niemals committen:

* `.env`-Dateien
* Geheimnisse
* API-Schlüssel
* Passwörter

`.gitignore`-Datei erstellen:

```bash
nano .gitignore
```

Eintragen:

```
.env
*.log
node_modules/
```

Speichern und beenden.

Falls `.env` bereits committed wurde:

```bash
git rm --cached .env
git commit -m ".env aus der Versionskontrolle entfernen"
```

Jetzt wird diese Datei dauerhaft ignoriert.

---

## 14. Praxisbeispiel: Serverkonfiguration zurückrollen

Stell dir vor, du bearbeitest `docker-compose.yml`.

1. Änderungen vornehmen
2. Testen
3. Falls kaputt → zurückrollen:

```bash
git restore docker-compose.yml
```

Oder zu einer bekannt funktionierenden Version zurückkehren:

```bash
git checkout COMMIT_ID docker-compose.yml
```

Genau deshalb ist Git so wertvoll für die Serververwaltung.

---

## 15. Nützliche tägliche Befehle

```bash
git status
git add .
git commit -m "Nachricht"
git log --oneline
git branch
git switch main
```

---

## 16. Goldene Regeln für Self-Hosting

* Committen, BEVOR du riskante Änderungen machst
* Niemals Geheimnisse committen
* Branches zum Experimentieren nutzen
* `git revert` statt `reset --hard` verwenden, wenn unsicher
* Git als Sicherheitsnetz betrachten

---

### Mentales Modell

Git ist nicht nur für Entwickler.

Es ist:

* Ein Backup-System
* Ein Änderungstracker
* Ein Rollback-Mechanismus
* Ein Sicherheitsmechanismus

Für Serveradministratoren gilt: Git = **kontrolliertes Experimentieren ohne Angst**.

---

## 17. Häufige Fehler

### Versehentlich Geheimnisse committen

Niemals `.env`-Dateien oder Passwörter committen. Falls es passiert ist: `git rm --cached` und `.gitignore` verwenden (siehe Abschnitt 13).

### Konflikte mit dem Remote

Wenn `git push` fehlschlägt, zuerst `git pull` ausführen und Konflikte auflösen.

### Große Verzeichnisse oder Dateien committen

Git kann zwar auch größere Dateien speichern, ist dafür aber nicht gedacht. Alle Versionen bleiben in der Historie erhalten, was das `.git`-Verzeichnis sehr groß macht. Das nachträgliche Entfernen aus der Historie ist sehr aufwendig.

Git eignet sich am besten für Skripte und Konfigurationsdateien. Selbst bei häufigen Änderungen wird das Repository kaum größer als ein paar Megabyte.

Besonders aufpassen solltest du bei Log-Dateien. Anwendungen können anfangen, Logs in das von Git verfolgte Verzeichnis zu schreiben. Logs wachsen schnell (mehrere Zeilen pro Sekunde) und können gigabytes groß werden. Außerdem können Logs sensible Informationen enthalten, die nicht in die Versionskontrolle oder auf einen Remote-Server gehören.
