# PGP / GPG - Spickzettel

## 1. Was ist PGP / GPG?

PGP steht für **Pretty Good Privacy** – ein System, das Dateien und Nachrichten verschlüsselt oder mit einer digitalen Signatur versieht. GPG (**GNU Privacy Guard**) ist die freie Open-Source-Umsetzung davon, die du auf Linux verwendest.

Zwei typische Anwendungsfälle:
- Du lädst eine Software herunter und willst prüfen, ob die Datei wirklich vom Hersteller stammt (Signaturprüfung).
- Du willst eine Datei verschlüsselt an jemanden schicken, sodass nur diese Person sie lesen kann.

**Windows-Analogie:** Auf Windows gibt es Gpg4win (mit der grafischen Oberfläche Kleopatra), das genau dieselbe Funktion übernimmt. GPG auf der Linux-Kommandozeile ist das Äquivalent dazu – ohne grafische Oberfläche, aber gleiche Konzepte.

GPG arbeitet mit einem **Schlüsselpaar**: einem öffentlichen Schlüssel, den du frei verteilst, und einem privaten Schlüssel, den nur du kennst. Was mit dem öffentlichen Schlüssel verschlüsselt wird, kann nur der private Schlüssel entschlüsseln – und umgekehrt.

> Hinweis: GPG kann auch zur E-Mail-Verschlüsselung genutzt werden (z. B. mit Thunderbird und dem Enigmail-Plugin). Das wird hier nicht vertieft.

## 2. GPG installieren

Auf Ubuntu / Debian ist GPG oft schon vorinstalliert. Prüfe das zuerst:

```bash
gpg --version
```

Wenn der Befehl nicht gefunden wird, installiere GPG:

```bash
sudo apt update
sudo apt install gnupg   # GPG installieren
```

## 3. Schlüsselpaar erstellen

Ein Schlüsselpaar besteht aus deinem öffentlichen und deinem privaten Schlüssel. Du erstellst beides in einem Schritt:

```bash
gpg --full-generate-key
```

GPG fragt dich interaktiv:

```
Bitte wählen Sie, welche Art von Schlüssel Sie möchten:
   (1) RSA and RSA
   (2) DSA and Elgamal
   ...
Ihre Auswahl? 1

RSA-Schlüssel können zwischen 1024 und 4096 Bit lang sein.
Welche Schlüssellänge wünschen Sie? 4096

Wie lange soll der Schlüssel gültig sein? 0 (kein Ablaufdatum)

Name: Max Mustermann
E-Mail: max@example.com
Kommentar: (leer lassen oder kurze Notiz)
```

Am Ende wirst du nach einem **Passwort (Passphrase)** für deinen privaten Schlüssel gefragt. Dieses Passwort schützt den Schlüssel, falls jemand Zugang zu deiner Schlüsseldatei bekommt. Wähle ein starkes Passwort und vergiss es nicht – es gibt keine Wiederherstellung.

## 4. Schlüssel auflisten

Alle öffentlichen Schlüssel in deinem Schlüsselbund anzeigen:

```bash
gpg --list-keys
```

Beispielausgabe:
```
pub   rsa4096 2026-03-12 [SC]
      A1B2C3D4E5F6A1B2C3D4E5F6A1B2C3D4E5F6A1B2
uid           [ultimate] Max Mustermann <max@example.com>
sub   rsa4096 2026-03-12 [E]
```

Deinen eigenen privaten Schlüssel anzeigen:

```bash
gpg --list-secret-keys   # Zeigt private Schlüssel an
```

> Die lange Zeichenkette (Fingerprint) identifiziert den Schlüssel eindeutig. Die letzten 8 oder 16 Zeichen nennt man **Key-ID** und wird oft als Kurzreferenz genutzt.

## 5. Öffentlichen Schlüssel exportieren und importieren

### Exportieren

Damit jemand dir eine verschlüsselte Datei schicken oder deine Signaturen prüfen kann, muss diese Person deinen öffentlichen Schlüssel haben. Exportiere ihn als Textdatei:

```bash
gpg --export --armor max@example.com > mein-public-key.asc
# --armor gibt den Schlüssel als lesbaren ASCII-Text aus
```

Die Datei `mein-public-key.asc` kannst du dann per E-Mail verschicken, auf einer Website veröffentlichen oder direkt weitergeben.

### Importieren

Wenn du den öffentlichen Schlüssel einer anderen Person hast (z. B. als `.asc`-Datei), importierst du ihn so:

```bash
gpg --import empfaenger-public-key.asc   # Schlüssel in den Schlüsselbund eintragen
```

Nach dem Import kannst du prüfen, ob der Schlüssel vorhanden ist:

```bash
gpg --list-keys   # Neuen Schlüssel in der Liste sehen
```

## 6. Datei verschlüsseln

Du kannst eine Datei verschlüsseln, sodass nur eine bestimmte Person (der Empfänger) sie entschlüsseln kann. Dafür brauchst du den **öffentlichen Schlüssel des Empfängers** (siehe Abschnitt 5).

```bash
gpg --encrypt --armor --recipient empfaenger@example.com geheim.txt
# --encrypt        Datei verschlüsseln
# --armor          Ausgabe als ASCII-Text (statt Binärdatei)
# --recipient      Empfänger angeben (per E-Mail oder Key-ID)
```

Das erzeugt die Datei `geheim.txt.asc`. Die Originaldatei bleibt erhalten – du kannst sie danach manuell löschen, wenn nötig.

Ohne `--armor` wird eine Binärdatei (`geheim.txt.gpg`) erzeugt:

```bash
gpg --encrypt --recipient empfaenger@example.com geheim.txt
```

## 7. Datei entschlüsseln

Wenn du eine verschlüsselte Datei erhalten hast und dein privater Schlüssel der Empfänger ist:

```bash
gpg --decrypt geheim.txt.asc > geheim-entschluesselt.txt
# GPG fragt nach deiner Passphrase und schreibt den Klartext in die Ausgabedatei
```

Oder direkt eine neue Datei ausgeben lassen:

```bash
gpg --output geheim-entschluesselt.txt --decrypt geheim.txt.asc
```

GPG erkennt automatisch, welcher private Schlüssel zum Entschlüsseln benötigt wird, und fragt nach der Passphrase.

## 8. Datei signieren und Signatur prüfen

### Datei signieren

Eine Signatur beweist, dass die Datei von dir stammt und nicht verändert wurde. Zum Signieren brauchst du deinen **privaten Schlüssel**.

Signatur als separate Datei erstellen (häufig bei Software-Downloads):

```bash
gpg --detach-sign --armor datei.tar.gz
# Erzeugt datei.tar.gz.asc – die Signaturdatei
# --detach-sign  Signatur separat von der Datei speichern
```

Alternativ: Datei und Signatur kombiniert speichern:

```bash
gpg --sign --armor datei.txt   # Erzeugt datei.txt.asc mit Inhalt + Signatur
```

### Signatur prüfen

Wenn du eine Datei und eine zugehörige Signaturdatei hast (z. B. nach einem Software-Download):

```bash
gpg --verify datei.tar.gz.asc datei.tar.gz
# Prüft, ob die Signatur zur Datei passt
```

Beispielausgabe bei gültiger Signatur:
```
gpg: Signature made Mo 12 Mär 2026 10:00:00 CET
gpg:                using RSA key A1B2C3D4E5F6A1B2
gpg: Good signature from "Max Mustermann <max@example.com>" [ultimate]
```

Wenn du den öffentlichen Schlüssel des Unterzeichners noch nicht importiert hast, siehst du stattdessen eine Warnung. Importiere zuerst den Schlüssel (Abschnitt 5), dann prüfe erneut.

## 9. Nützliche Befehle – Übersicht

| Befehl | Was er macht |
|--------|--------------|
| `gpg --version` | Installierte GPG-Version anzeigen |
| `gpg --full-generate-key` | Neues Schlüsselpaar erstellen |
| `gpg --list-keys` | Alle öffentlichen Schlüssel auflisten |
| `gpg --list-secret-keys` | Alle privaten Schlüssel auflisten |
| `gpg --export --armor <E-Mail> > key.asc` | Öffentlichen Schlüssel exportieren |
| `gpg --import key.asc` | Öffentlichen Schlüssel importieren |
| `gpg --encrypt --armor --recipient <E-Mail> <Datei>` | Datei verschlüsseln |
| `gpg --decrypt <Datei>.asc > <Ausgabe>` | Datei entschlüsseln |
| `gpg --detach-sign --armor <Datei>` | Separate Signaturdatei erstellen |
| `gpg --sign --armor <Datei>` | Datei mit Signatur kombiniert speichern |
| `gpg --verify <Signatur>.asc <Datei>` | Signatur prüfen |
| `gpg --delete-key <E-Mail>` | Öffentlichen Schlüssel löschen |
| `gpg --delete-secret-key <E-Mail>` | Privaten Schlüssel löschen |

## 10. Häufige Fehler

**Problem:** `gpg: keyserver receive failed` oder ähnliche Fehler beim Importieren.
**Lösung:** Du importierst über einen Keyserver, was hier nicht behandelt wird. Lade die `.asc`-Datei direkt herunter und importiere sie mit `gpg --import`.

**Problem:** `gpg: decryption failed: No secret key` beim Entschlüsseln.
**Lösung:** Dein privater Schlüssel ist nicht der Empfänger dieser verschlüsselten Datei. Die Datei wurde für jemand anderen verschlüsselt, oder du verwendest das falsche GPG-Schlüsselbund.

**Problem:** `gpg: BAD signature` beim Prüfen einer Signatur.
**Lösung:** Die Datei wurde nach dem Signieren verändert (oder ist beschädigt). Lade sie erneut herunter und prüfe die Signatur nochmals.

**Problem:** `gpg: Can't check signature: No public key` beim Signaturprüfen.
**Lösung:** Du hast den öffentlichen Schlüssel des Unterzeichners noch nicht importiert. Beschaffe die `.asc`-Datei mit dem öffentlichen Schlüssel und importiere sie zuerst (Abschnitt 5).

**Problem:** Die Passphrase wird abgelehnt, obwohl sie korrekt ist.
**Lösung:** Prüfe, ob du die richtige Tastaturlayout-Einstellung verwendest. Groß-/Kleinschreibung und Sonderzeichen können je nach Layout abweichen. Teste die Passphrase in einem Texteditor, bevor du sie eingibst.

**Problem:** Du willst eine Datei für dich selbst verschlüsseln (z. B. als Backup).
**Lösung:** Gib deine eigene E-Mail-Adresse als Empfänger an:
```bash
gpg --encrypt --armor --recipient meine@email.com wichtige-datei.txt
```
