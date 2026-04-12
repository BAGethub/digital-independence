# curl und wget - Spickzettel

`curl` und `wget` sind Kommandozeilen-Werkzeuge zum Herunterladen von Dateien und Kommunizieren mit Webservern. Beide sind auf Ubuntu vorinstalliert und im Self-Hosting unverzichtbar.

**Windows-Analogie:**
- `curl` ist wie der Browser-Entwicklermodus (Netzwerk-Tab) – du siehst und steuerst jeden HTTP-Request.
- `wget` ist wie "Speichern unter" im Browser – simpel, für Downloads gedacht.

---

## 1. curl Grundlagen

### Einfacher GET-Request

```bash
curl https://example.com               # Inhalt der Seite ausgeben
curl -s https://example.com            # Ohne Fortschrittsbalken (silent)
curl -i https://example.com            # Mit HTTP-Headern in der Ausgabe
curl -I https://example.com            # Nur Header anzeigen (HEAD-Request)
```

### Antwort in eine Datei speichern

```bash
curl -o ausgabe.html https://example.com        # In "ausgabe.html" speichern
curl -O https://example.com/datei.zip           # Originalname beibehalten
```

Unterschied:
- `-o dateiname` – du bestimmst den Dateinamen
- `-O` – Dateiname aus der URL übernommen

---

## 2. Weiterleitungen folgen (-L)

Viele URLs leiten auf eine andere URL weiter (HTTP 301/302). Standardmäßig folgt curl diesen Weiterleitungen nicht.

```bash
curl https://github.com/user/repo/releases/latest/download/tool.tar.gz
# → Gibt nur "301 Moved" zurück, lädt nichts herunter

curl -L https://github.com/user/repo/releases/latest/download/tool.tar.gz
# → Folgt der Weiterleitung und lädt die Datei herunter
```

**Mit Datei speichern kombinieren:**
```bash
curl -L -o tool.tar.gz https://github.com/user/repo/releases/latest/download/tool.tar.gz
```

---

## 3. HTTP-Header setzen

Header sind Metainformationen, die du mit jedem Request mitsendest.

```bash
# Content-Type setzen (wichtig für APIs)
curl -H "Content-Type: application/json" https://api.example.com/data

# Mehrere Header
curl -H "Content-Type: application/json" \
     -H "Accept: application/json" \
     https://api.example.com/data

# Authorization-Header (z.B. für APIs mit API-Key)
curl -H "Authorization: Bearer DEIN_TOKEN" https://api.example.com/data
```

---

## 4. POST-Requests

```bash
# Formular-Daten senden
curl -X POST -d "name=Alice&email=alice@example.com" https://example.com/form

# JSON-Daten senden (für REST-APIs)
curl -X POST \
     -H "Content-Type: application/json" \
     -d '{"name": "Alice", "email": "alice@example.com"}' \
     https://api.example.com/users

# JSON aus einer Datei senden
curl -X POST \
     -H "Content-Type: application/json" \
     -d @payload.json \
     https://api.example.com/users
```

---

## 5. curl für API-Tests

curl ist perfekt, um REST-APIs direkt aus dem Terminal zu testen – ohne Postman oder Browser.

### GET mit JSON-Antwort

```bash
curl -s https://api.github.com/users/torvalds | python3 -m json.tool
```

Die Ausgabe durch `python3 -m json.tool` leiten formatiert das JSON lesbar.

### PUT-Request

```bash
curl -X PUT \
     -H "Content-Type: application/json" \
     -d '{"status": "active"}' \
     https://api.example.com/users/42
```

### DELETE-Request

```bash
curl -X DELETE https://api.example.com/users/42
```

### Authentifizierung testen

```bash
# Basic Auth (Benutzername:Passwort)
curl -u alice:meinpasswort https://api.example.com/geschuetzt

# API-Key als URL-Parameter
curl "https://api.example.com/data?api_key=MEIN_KEY"
```

### HTTP-Statuscode prüfen

```bash
curl -s -o /dev/null -w "%{http_code}" https://example.com
# Ausgabe: 200
```

---

## 6. wget für Downloads

`wget` ist einfacher als curl und speziell für Downloads optimiert.

```bash
wget https://example.com/datei.zip              # Datei herunterladen
wget -O anderer-name.zip https://example.com/datei.zip   # Mit eigenem Namen
wget -q https://example.com/datei.zip           # Ruhig (kein Output)
wget -c https://example.com/grosse-datei.zip    # Unterbrochenen Download fortsetzen
```

### Rekursiver Download

```bash
wget -r -np https://example.com/ordner/    # Ganzen Ordner herunterladen
```

### Im Hintergrund herunterladen

```bash
wget -b https://example.com/riesige-datei.iso
# Download läuft im Hintergrund, Fortschritt in wget-log
tail -f wget-log
```

---

## 7. curl vs. wget – Wann was benutzen?

| Aufgabe | Empfehlung |
|---|---|
| API testen | `curl` |
| Einzelne Datei herunterladen | `wget` oder `curl -O` |
| Mit Redirects umgehen | `curl -L` |
| Download fortsetzen | `wget -c` |
| JSON-Daten senden | `curl` |
| Ordner rekursiv herunterladen | `wget -r` |

---

## 8. Warum `curl | bash` gefährlich ist

Du siehst oft Installationsanleitungen wie:

```bash
# VORSICHT - niemals blind ausführen!
curl https://get.example.com/install.sh | bash
```

**Was hier passiert:** Das Script wird heruntergeladen und sofort als Root ausgeführt – ohne dass du gesehen hast, was darin steht.

**Das Problem:**
- Du vertraust blindlings dem Inhalt der URL.
- Die URL könnte gehackt worden sein und Malware liefern.
- Du hast keine Chance, das Script vorher zu prüfen.
- Mit `sudo curl ... | bash` hast du dem Script vollen Root-Zugriff gegeben.

### Sicherer Alternativ-Workflow

**Schritt 1: Script herunterladen**
```bash
curl -o install.sh https://get.example.com/install.sh
```

**Schritt 2: Script lesen**
```bash
nano install.sh
# oder
less install.sh
```

**Schritt 3: Script nur ausführen, wenn du es verstanden hast**
```bash
chmod +x install.sh
bash install.sh
```

**Oder noch besser – prüfe Checksums:**
```bash
# Viele seriöse Projekte veröffentlichen SHA256-Hashes
curl -O https://example.com/tool.tar.gz
curl -O https://example.com/tool.tar.gz.sha256

sha256sum -c tool.tar.gz.sha256
# Ausgabe: tool.tar.gz: OK  ← nur dann weitermachen
```

**Faustregel:** Je mehr Rechte ein Script braucht, desto genauer solltest du es lesen.

---

## 9. Nützliche curl-Optionen im Überblick

| Option | Bedeutung |
|---|---|
| `-s` | Silent – kein Fortschrittsbalken |
| `-o datei` | In Datei speichern (eigener Name) |
| `-O` | In Datei speichern (URL-Name) |
| `-L` | Weiterleitungen folgen |
| `-i` | Response-Header mit ausgeben |
| `-I` | Nur Header ausgeben (HEAD) |
| `-X POST` | HTTP-Methode setzen |
| `-H "..."` | Header hinzufügen |
| `-d "..."` | Request-Body senden |
| `-u user:pass` | Basic Authentication |
| `-v` | Verbose – alle Details anzeigen |
| `--max-time 10` | Timeout auf 10 Sekunden setzen |
| `-k` | SSL-Zertifikat nicht prüfen (nur für Tests!) |

---

## 10. Troubleshooting und Tipps

- **SSL-Fehler:** Systemzeit falsch gestellt? `timedatectl` prüfen. Oder `-k` zum Testen (nie in Produktion).
- **Weiterleitung wird nicht befolgt:** `-L` vergessen?
- **API antwortet mit 401:** Authentifizierung fehlt oder Token abgelaufen.
- **Große Datei bricht ab:** `wget -c` zum Fortsetzen verwenden.
- **JSON nicht lesbar:** Output durch `| python3 -m json.tool` oder `| jq .` leiten (`jq` muss installiert sein: `sudo apt install jq`).
- **curl nicht installiert:** `sudo apt install curl`
- **wget nicht installiert:** `sudo apt install wget`
