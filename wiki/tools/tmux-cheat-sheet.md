# tmux - Spickzettel

tmux ist ein "Terminal-Multiplexer". Er erlaubt es, mehrere Terminals in einem Fenster zu betreiben und – besonders wichtig beim Self-Hosting – SSH-Sessions am Leben zu halten, auch wenn die Verbindung unterbrochen wird.

**Windows-Analogie:** Stell dir vor, du arbeitest in einem Remote-Desktop. Wenn du das Fenster schließt, laufen alle Programme weiter. tmux tut dasselbe für Terminal-Sessions über SSH.

---

## 1. Warum tmux?

**Das Problem ohne tmux:**
- Du verbindest dich per SSH mit deinem Server.
- Du startest einen langen Prozess (z.B. Backup, Datenbank-Migration).
- Deine Internetverbindung bricht ab.
- Der Prozess stirbt sofort – SSH-Session weg, Prozess weg.

**Die Lösung mit tmux:**
- Prozesse laufen in tmux-Sessions auf dem Server.
- Du kannst die Session verlassen ("detachen") und später wieder anhängen ("attachen").
- Auch nach einem SSH-Verbindungsabbruch läuft alles weiter.

---

## 2. tmux installieren

```bash
sudo apt install tmux
```

---

## 3. Konzepte: Sessions, Windows, Panes

tmux hat drei Ebenen:

```
Server
└── Session (z.B. "backup")
    ├── Window 1 (z.B. "logs")
    │   ├── Pane links
    │   └── Pane rechts
    └── Window 2 (z.B. "monitoring")
        └── Pane (ganzes Fenster)
```

- **Session:** Ein unabhängiger Arbeitsbereich. Du kannst dich davon trennen und wieder verbinden.
- **Window:** Ein Tab innerhalb einer Session (wie Browser-Tabs).
- **Pane:** Ein geteilter Bereich innerhalb eines Windows (wie Splits im Editor).

---

## 4. Der Prefix-Key: Strg+B

Alle tmux-Befehle beginnen mit dem **Prefix**: `Strg+B`

Das heißt: Strg+B drücken, loslassen, dann den eigentlichen Befehl drücken.

**Beispiel:** Neues Window öffnen = `Strg+B`, dann `c`

---

## 5. Sessions verwalten

### Neue Session starten

```bash
tmux                          # Neue Session ohne Namen
tmux new -s backup            # Neue Session mit Namen "backup"
tmux new-session -s monitoring   # Gleich, ausgeschrieben
```

### Session verlassen (detachen) – Prozesse laufen weiter!

```
Strg+B, dann d
```

Du bist jetzt "draußen", aber die Session läuft auf dem Server weiter.

### Laufende Sessions anzeigen

```bash
tmux ls
# Ausgabe:
# backup: 1 windows (created Mon Mar 1 10:00:00 2026)
# monitoring: 2 windows (created Mon Mar 1 09:30:00 2026)
```

### Session wieder anhängen (attachen)

```bash
tmux attach -t backup         # Session "backup" anhängen
tmux a -t backup              # Kurzform
tmux a                        # Letzte Session anhängen (wenn nur eine)
```

### Session umbenennen

```
Strg+B, dann $
→ Neuen Namen eingeben, Enter
```

### Session beenden

```bash
tmux kill-session -t backup   # Session "backup" beenden
```

Oder innerhalb der Session: einfach alle offenen Shells mit `exit` schließen.

---

## 6. Windows (Tabs) verwalten

| Tastenkürzel | Aktion |
|---|---|
| `Strg+B`, `c` | Neues Window erstellen |
| `Strg+B`, `n` | Nächstes Window |
| `Strg+B`, `p` | Vorheriges Window |
| `Strg+B`, `0`–`9` | Zu Window Nummer springen |
| `Strg+B`, `,` | Aktuelles Window umbenennen |
| `Strg+B`, `w` | Alle Windows als Liste anzeigen |
| `Strg+B`, `&` | Aktuelles Window schließen (mit Bestätigung) |

**Windows in der Statusleiste:** Am unteren Rand siehst du alle offenen Windows. Das aktive ist mit `*` markiert:

```
[backup] 0:bash* 1:logs  2:monitoring
```

---

## 7. Panes (geteilte Fenster) verwalten

| Tastenkürzel | Aktion |
|---|---|
| `Strg+B`, `%` | Pane vertikal teilen (links/rechts) |
| `Strg+B`, `"` | Pane horizontal teilen (oben/unten) |
| `Strg+B`, Pfeiltasten | Zwischen Panes wechseln |
| `Strg+B`, `x` | Aktuellen Pane schließen |
| `Strg+B`, `z` | Pane auf ganzes Fenster vergrößern (Toggle) |
| `Strg+B`, `{` | Pane nach links verschieben |
| `Strg+B`, `}` | Pane nach rechts verschieben |

**Typisches Setup für Monitoring:**
```bash
tmux new -s monitoring
# Pane teilen:
# Strg+B, "  → oben: Logs, unten: System-Status
```

---

## 8. Scrollback-Modus (in der Ausgabe scrollen)

Im Terminal kann man normalerweise nicht in der Ausgabe hochscrollen, wenn tmux aktiv ist. Der Scrollback-Modus löst das.

**Scrollback-Modus aktivieren:**
```
Strg+B, dann [
```

Jetzt kannst du mit den Pfeiltasten oder `Page Up`/`Page Down` scrollen.

**Scrollback-Modus beenden:**
```
q   oder   Enter
```

**Suchen im Scrollback:**
```
Strg+B, [   → Scrollback-Modus
/           → Suchen (nach unten)
?           → Suchen (nach oben)
n           → Nächsten Treffer
```

---

## 9. Praktische Beispiele

### Server-Aufgabe absichern

```bash
# SSH-Verbindung aufbauen
ssh user@mein-server.de

# tmux-Session starten
tmux new -s deployment

# Langen Befehl starten
./deploy.sh

# Session verlassen (deploy.sh läuft weiter!)
# Strg+B, d

# Verbindung trennen – alles läuft weiter!
exit

# Später wieder verbinden
ssh user@mein-server.de
tmux attach -t deployment
```

### Logs und Shell gleichzeitig anzeigen

```bash
tmux new -s debug
# Strg+B, "  → Fenster teilen

# Im oberen Pane:
tail -f /var/log/nginx/error.log

# Strg+B, Pfeil nach unten → zum unteren Pane
# Im unteren Pane:
curl https://meine-seite.de
```

---

## 10. tmux.conf – Konfiguration anpassen

Die Konfigurationsdatei liegt unter `~/.tmux.conf`. Änderungen werden nach dem Neustart von tmux wirksam, oder sofort mit:

```bash
tmux source-file ~/.tmux.conf
```

**Empfohlene Grundkonfiguration:**

```bash
nano ~/.tmux.conf
```

Folgendes einfügen:

```
# Mausbedienung aktivieren (Scrollen, Pane-Wechsel)
set -g mouse on

# Scrollback-Buffer vergrößern (Standard ist 2000 Zeilen)
set -g history-limit 10000

# Statusleiste anpassen
set -g status-bg black
set -g status-fg white

# Fensternummerierung bei 1 beginnen (statt 0)
set -g base-index 1

# Pane-Nummerierung ebenfalls bei 1 beginnen
setw -g pane-base-index 1

# Verzögerung nach Prefix reduzieren
set -sg escape-time 1
```

**Maus aktivieren** ist besonders für Anfänger nützlich: Du kannst dann per Mausklick zwischen Panes wechseln und mit dem Mausrad scrollen.

---

## 11. Übersicht aller wichtigen Shortcuts

### Session-Shortcuts

| Tastenkürzel | Aktion |
|---|---|
| `Strg+B`, `d` | Session verlassen (detach) |
| `Strg+B`, `$` | Session umbenennen |
| `Strg+B`, `s` | Alle Sessions auflisten |

### Window-Shortcuts

| Tastenkürzel | Aktion |
|---|---|
| `Strg+B`, `c` | Neues Window |
| `Strg+B`, `n` | Nächstes Window |
| `Strg+B`, `p` | Vorheriges Window |
| `Strg+B`, `,` | Window umbenennen |
| `Strg+B`, `w` | Window-Liste |

### Pane-Shortcuts

| Tastenkürzel | Aktion |
|---|---|
| `Strg+B`, `%` | Vertikal teilen |
| `Strg+B`, `"` | Horizontal teilen |
| `Strg+B`, Pfeiltasten | Pane wechseln |
| `Strg+B`, `z` | Pane maximieren/minimieren |
| `Strg+B`, `x` | Pane schließen |

### Sonstige Shortcuts

| Tastenkürzel | Aktion |
|---|---|
| `Strg+B`, `[` | Scrollback-Modus |
| `Strg+B`, `?` | Alle Shortcuts anzeigen |
| `Strg+B`, `:` | tmux-Befehlseingabe |

---

## 12. Troubleshooting und Tipps

- **tmux nicht gefunden:** `sudo apt install tmux`
- **Kann keine Session finden:** `tmux ls` zeigt alle laufenden Sessions. Vielleicht hast du einen anderen Servernamen?
- **Prefix-Taste geändert:** Manche setzen den Prefix auf `Strg+A` (wie `screen`). Steht dann in `~/.tmux.conf`.
- **Farben stimmen nicht:** `set -g default-terminal "screen-256color"` in `~/.tmux.conf` hinzufügen.
- **Mauszeiger funktioniert nicht:** `set -g mouse on` in `~/.tmux.conf` und Konfiguration neu laden.
- **Session stirbt doch:** Prüfe ob der Server selbst neu gestartet wurde – dann ist alles weg. Für permanente Prozesse: `systemd`-Services nutzen.

---

## 13. tmux vs. screen

`screen` ist ein älteres, ähnliches Tool. tmux ist moderner und für neue Installationen empfohlen. Falls ein Tutorial `screen` verwendet, lassen sich die meisten Konzepte direkt auf tmux übertragen.
