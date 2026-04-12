# Nano - Spickzettel

Nano ist ein einfacher Texteditor, der direkt im Terminal läuft. Kein GUI, kein Mausbedarf – nur Tastatur. Für viele Server-Aufgaben ist das genau richtig.

**Windows-Analogie:** Nano ist wie Notepad, aber im Terminal. Kein Word, kein Formatieren – nur Text bearbeiten und speichern.

---

## 1. Nano öffnen

```bash
nano dateiname.txt          # Datei öffnen (wird erstellt, wenn sie nicht existiert)
nano /etc/hosts             # Systemdatei öffnen (mit sudo falls nötig)
sudo nano /etc/ssh/sshd_config   # Datei mit Root-Rechten öffnen
```

Nano öffnet sich direkt im Terminal. Am unteren Rand siehst du die verfügbaren Tastenkürzel.

---

## 2. Die Nano-Oberfläche verstehen

```
  GNU nano 7.2                dateiname.txt

  [Hier steht dein Text]




^G Hilfe     ^O Speichern   ^W Suchen    ^K Ausschneiden
^X Beenden   ^R Einfügen    ^\ Ersetzen  ^U Einfügen
```

- `^` bedeutet die **Strg-Taste** (Control)
- Die Leiste unten zeigt die wichtigsten Befehle
- `M-` bedeutet **Alt-Taste** (Meta)

---

## 3. Speichern und Beenden

| Tastenkürzel | Aktion |
|---|---|
| `Strg+O` | Datei speichern ("Write Out") |
| `Strg+X` | Nano beenden |
| `Strg+X`, dann `J` | Beenden und speichern (Ja, auf deutschsprachigen Systemen) |
| `Strg+X`, dann `N` | Beenden ohne speichern (Nein / No) |

**Typischer Ablauf:**
```
1. Text bearbeiten
2. Strg+O drücken → Dateiname bestätigen (Enter)
3. Strg+X drücken → Nano beenden
```

Wenn du ungespeicherte Änderungen hast und `Strg+X` drückst, fragt Nano:
```
Save modified buffer?  (Answering "No" will DISCARD changes.)
 Y Yes
 N No           ^C Cancel
```

---

## 4. Navigieren

| Tastenkürzel | Aktion |
|---|---|
| Pfeiltasten | Cursor bewegen |
| `Strg+A` | Zum Zeilenanfang |
| `Strg+E` | Zum Zeilenende |
| `Strg+Y` | Eine Seite hoch (Page Up) |
| `Strg+V` | Eine Seite runter (Page Down) |
| `Strg+_` | Zu einer bestimmten Zeile springen |

**Zu einer bestimmten Zeile springen:**
```
Strg+_ drücken → Zeilennummer eingeben → Enter
```

Beispiel: `Strg+_`, dann `42`, dann `Enter` springt zu Zeile 42. Nützlich, wenn eine Fehlermeldung auf eine bestimmte Zeile in einer Konfigdatei verweist.

---

## 5. Suchen

```
Strg+W drücken → Suchbegriff eingeben → Enter
```

- Drücke nochmals `Strg+W` + `Enter`, um zum nächsten Treffer zu springen.
- Die Suche ist standardmäßig nicht case-sensitiv.

**Optionen beim Suchen:**
```
^W  Suchen
^C  Abbrechen
M-C Case-sensitiv umschalten
M-R Reguläre Ausdrücke umschalten
```

---

## 6. Suchen und Ersetzen

```
Strg+\ drücken → Suchbegriff eingeben → Enter → Ersetzung eingeben → Enter
```

Dann wählen:
- `Y` – diesen Treffer ersetzen
- `N` – diesen Treffer überspringen
- `A` – alle Treffer ersetzen
- `^C` – Abbrechen

**Beispiel:** Alle Vorkommen von `localhost` durch `127.0.0.1` ersetzen:
```
Strg+\ → localhost → Enter → 127.0.0.1 → Enter → A
```

---

## 7. Ausschneiden, Kopieren, Einfügen

| Tastenkürzel | Aktion |
|---|---|
| `Strg+K` | Aktuelle Zeile ausschneiden |
| `Strg+U` | Ausgeschnittene Zeile einfügen |
| `Strg+6` | Markierung beginnen (dann mit Pfeiltasten auswählen) |
| `Alt+6` | Markierten Text kopieren (ohne ausschneiden) |

**Zeile verschieben (Cut & Paste):**
```
1. Cursor auf die Zeile bewegen
2. Strg+K → Zeile ausschneiden
3. Cursor zur Zielposition bewegen
4. Strg+U → Zeile einfügen
```

**Mehrere Zeilen ausschneiden:**
```
Strg+K mehrfach drücken → alle ausgeschnittenen Zeilen sammeln sich
Strg+U → alles auf einmal einfügen
```

---

## 8. Rückgängig und Wiederholen

| Tastenkürzel | Aktion |
|---|---|
| `Alt+U` | Letzte Aktion rückgängig machen |
| `Alt+E` | Rückgängig gemachte Aktion wiederholen |

---

## 9. Syntax-Highlighting aktivieren

Nano kann Code einfärben (Python, Bash, Konfigdateien usw.). Auf Ubuntu 24.04 ist das oft schon aktiviert.

**Prüfen ob Highlighting aktiv ist:**
```bash
nano --syntax=sh meinscript.sh
```

**In der Nano-Konfigdatei aktivieren:**
```bash
nano ~/.nanorc
```

Folgendes einfügen:
```
include "/usr/share/nano/*.nanorc"
```

Speichern und neu öffnen – jetzt sollte Code eingefärbt sein.

**Systemweit (alle Benutzer):**
```bash
sudo nano /etc/nanorc
```
Die Zeile `# include "/usr/share/nano/*.nanorc"` finden und das `#` am Anfang entfernen.

**Verfügbare Syntax-Definitionen anzeigen:**
```bash
ls /usr/share/nano/
```

---

## 10. Nützliche Nano-Optionen beim Starten

```bash
nano -l datei.py          # Zeilennummern anzeigen
nano +42 datei.txt        # Direkt zu Zeile 42 springen
nano -v datei.txt         # Read-only Modus (kein versehentliches Bearbeiten)
nano -w datei.txt         # Langen Zeilen nicht umbrechen
```

---

## 11. Komplettes Tastenkürzel-Übersicht

| Tastenkürzel | Aktion |
|---|---|
| `Strg+O` | Speichern |
| `Strg+X` | Beenden |
| `Strg+W` | Suchen |
| `Strg+\` | Suchen und Ersetzen |
| `Strg+K` | Zeile ausschneiden |
| `Strg+U` | Einfügen |
| `Strg+_` | Zu Zeile springen |
| `Strg+G` | Hilfe anzeigen |
| `Strg+C` | Aktuelle Zeilennummer anzeigen |
| `Alt+U` | Rückgängig |
| `Alt+E` | Wiederholen |
| `Strg+Y` | Seite hoch |
| `Strg+V` | Seite runter |

---

## 12. Troubleshooting und Tipps

- **"Permission denied" beim Speichern:** Du hast die Datei ohne `sudo` geöffnet. Beenden mit `Strg+X`, nochmal mit `sudo nano` öffnen.
- **Nano hängt oder verhält sich komisch:** `Strg+C` drücken, um laufende Aktionen abzubrechen.
- **Versehentlich beendet ohne zu speichern:** Nano fragt immer nach – lies die Frage genau!
- **Kein Syntax-Highlighting:** `include "/usr/share/nano/*.nanorc"` in `~/.nanorc` eintragen.
- **Alternative zu Nano:** `vim` ist mächtiger, aber hat eine steile Lernkurve. Für Anfänger ist Nano ideal.
