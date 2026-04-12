# Szenario: Self-Hosted Nextcloud mit Docker Compose + Git

Voraussetzungen:

* Ubuntu Server 24.04 LTS
* Git bereits installiert
* Docker + Docker Compose Plugin installiert
* Git-Repo bereits initialisiert

Projektstruktur:

```
/srv/nextcloud/
├── docker-compose.yml
├── .env
├── .gitignore
└── data/
```

---

## 1. `.env` zu `.gitignore` hinzufügen

`.gitignore` bearbeiten:

```bash
nano .gitignore
```

Eintragen:

```
.env
```

Speichern und beenden.

Falls `.env` bereits früher committet wurde:

```bash
git rm --cached .env
git commit -m ".env nicht mehr tracken"
```

Jetzt wird die Datei dauerhaft ignoriert.

---

## 2. `.env`-Datei absichern (SEHR WICHTIG)

Die `.env`-Datei enthält Geheimnisse:

```env
MYSQL_ROOT_PASSWORD=SuperGeheim
MYSQL_PASSWORD=NochEinGeheimnis
NEXTCLOUD_ADMIN_PASSWORD=AdminGeheimnis
```

### Berechtigungen korrekt setzen:

```bash
sudo chown root:root .env
sudo chmod 600 .env
```

Erklärung:

* `root:root` → Nur root besitzt die Datei
* `600` → Nur root kann lesen/schreiben
* Kein anderer Benutzer hat Zugriff

Prüfen:

```bash
ls -l .env
```

Sollte zeigen:

```
-rw------- 1 root root
```

⚠️ Die `.env`-Datei niemals für alle lesbar machen.

---

## 3. Baseline-Commit (bekannt funktionierender Zustand)

```bash
git add .
git commit -m "Funktionierendes Nextcloud-Grundsetup"
```

Zur Sicherheit taggen:

```bash
git tag stable-1
```

Das gibt dir einen dauerhaften Wiederherstellungspunkt.

---

## 4. Beispiel einer Änderung, die das System kaputt macht

Ursprüngliche funktionierende `docker-compose.yml`:

```yaml
services:
  app:
    image: nextcloud:27
    restart: unless-stopped
    env_file:
      - .env
    ports:
      - "8080:80"
```

Jemand führt ein "Upgrade" durch:

```yaml
image: nextcloud:28
```

Committen:

```bash
git add docker-compose.yml
git commit -m "Upgrade auf Nextcloud 28"
```

Deployen:

```bash
docker compose up -d
```

System ist kaputt 😬

---

## 5. FALSCHER Weg zum Zurückrollen (häufiger Anfängerfehler)

```bash
git checkout stable-1
```

Das führt zu:

* Detached HEAD
* Du bist nicht mehr auf `main`
* Der nächste Commit sorgt für Verwirrung
* Git beschwert sich später

Das sollte man vermeiden.

---

## 6. RICHTIGER Weg zum Zurückrollen (sauber & sicher)

Es gibt **zwei korrekte Methoden**.

---

## METHODE A (Empfohlen für Produktion): `git revert`

Das hält die Historie sauber und vermeidet Detached HEAD.

---

### Schritt 1: Den fehlerhaften Commit finden

```bash
git log --oneline
```

Beispiel:

```
abc1234 Upgrade auf Nextcloud 28
def5678 Funktionierendes Nextcloud-Grundsetup
```

---

### Schritt 2: Rückgängig machen

```bash
git revert abc1234
```

Git erstellt einen NEUEN Commit, der die Änderung rückgängig macht.

Du bleibst auf `main`.

Jetzt deployen:

```bash
docker compose down
docker compose up -d
```

System kehrt in den funktionierenden Zustand zurück.

✅ Kein Detached HEAD
✅ Keine zukünftige Git-Verwirrung
✅ Saubere Historie

Das ist der sicherste Ansatz.

---

## METHODE B (Notfall-Hard-Reset – nur wenn du weißt was du tust)

Nur verwenden wenn:

* Du sicher bist, dass niemand den fehlerhaften Commit gepullt hat
* Es sich um ein privates Repo handelt
* Du verstehst, was History-Rewriting bedeutet

---

### Schritt 1: Auf bekannt guten Commit zurücksetzen

```bash
git reset --hard def5678
```

Jetzt entspricht dein Arbeitsverzeichnis diesem Commit.

ABER:

Du schreibst die Historie um.

---

### Schritt 2: Docker-Status bereinigen

Sehr wichtig.

Git zurücksetzen setzt keine laufenden Container zurück.

Ausführen:

```bash
docker compose down
docker compose up -d --force-recreate
```

Falls Images geändert wurden:

```bash
docker compose pull
docker compose up -d --force-recreate
```

---

### Schritt 3: Falls ein Remote existiert

Falls du den fehlerhaften Commit bereits gepusht hast:

```bash
git push --force
```

⚠️ Das überschreibt die Remote-Historie. In Teams gefährlich.

---

## 7. Warum `git checkout <commit>` gefährlich ist

Wenn du:

```bash
git checkout def5678
```

ausführst, befindest du dich im **Detached-HEAD**-Zustand.

Wenn du jetzt committierst:

* Dieser Commit ist NICHT auf main
* Du bist "außerhalb der Zeitlinie"
* Das Zurückwechseln sorgt später für Verwirrung

Git warnt dich aus gutem Grund.

---

## 8. Alte Versionen sicher inspizieren

Wenn du nur einen früheren Zustand anschauen möchtest:

```bash
git checkout -b debug-alt def5678
```

Das erstellt einen Branch statt den HEAD zu lösen.

Jetzt bist du auf der sicheren Seite.

---

## 9. Reale Rollback-Strategie für die Produktion

Diese Regel anwenden:

1. Vor jeder riskanten Änderung committen
2. Stabile Releases taggen
3. `git revert` für Rollbacks verwenden
4. Container danach korrekt neu starten
5. Niemals Geheimnisse committen
6. Niemals `--force` verwenden, wenn du es nicht verstehst

---

## 10. Vollständiges sicheres Rollback-Beispiel

Fehlerhaftes Deployment passiert.

So vorgehen:

```bash
git log --oneline
git revert <fehlerhafter_commit>
docker compose down
docker compose up -d
```

Fertig.

System wiederhergestellt.
Historie sauber.
Kein Detached HEAD.
Keine Git-Beschwerden.

---

## 11. Extra-Sicherheitstrick (sehr praktisch)

Vor riskanten Änderungen:

```bash
git commit -am "Snapshot vor dem Upgrade"
git tag vor-upgrade
```

Dann upgraden.

Falls etwas schiefläuft:

```bash
git revert HEAD
docker compose down
docker compose up -d
```

Oder:

```bash
git reset --hard vor-upgrade
docker compose down
docker compose up -d --force-recreate
```

---

## 12. Wichtige Erkenntnis

Git verwaltet nur Dateien.

Docker verwaltet Container.

Für ein echtes Rollback musst du:

1. Dateien über Git wiederherstellen
2. Container über Docker neu erstellen
3. Manchmal Volumes entfernen, falls sich das Datenbankschema geändert hat

Git-Rollback allein reicht nicht aus.

---

### Mentales Modell

Git = Konfigurationshistorie
Docker = Laufzeitstatus

Beide müssen aufeinander abgestimmt sein.

---
