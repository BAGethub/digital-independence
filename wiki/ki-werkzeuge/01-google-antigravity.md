# Google Antigravity – kritisch eingeordnet

**Quelle:** [Google Antigravity is my secret productivity weapon that everyone should know about](https://www.xda-developers.com/google-antigravity-secret-productivity-weapon-that-everyone-should-know/) (XDA Developers)

Der XDA-Artikel empfiehlt Google Antigravity als Produktivitätswerkzeug. Die Empfehlung ist
nachvollziehbar, aber sie stellt eine Frage nicht, die für diesen Kurs zentral ist: Was gibst
du dafür ab? Diese Seite fasst beides zusammen.

---

## Was ist Antigravity?

Antigravity ist eine **agentische Entwicklungsumgebung**, die Google im November 2025
zusammen mit Gemini 3 Pro veröffentlicht hat. Technisch ist es ein Fork von VS Code, der
VS-Code-Erweiterungen unterstützt. Der Unterschied zu klassischen Autovervollständigungs-Tools
wie GitHub Copilot liegt im Anspruch: Antigravity soll nicht einzelne Zeilen ergänzen, sondern
**ganze Aufgaben eigenständig abarbeiten**.

Zwei Bausteine sind dafür zentral:

- **Manager View:** Du arbeitest nicht in einer Datei, sondern koordinierst mehrere Agenten
  gleichzeitig, etwa je einen für Backend, Frontend und Tests. Die Rolle verschiebt sich vom
  Schreiben zum Beauftragen und Prüfen.
- **Eingebauter Browser:** Der Agent startet die Anwendung selbst, klickt durch die Oberfläche
  und zeichnet das Ergebnis auf, um seine eigene Arbeit zu verifizieren.

## Was der Artikel beschreibt

Der Autor schildert als Beispiel den Bau eines Habit-Tracker-Dashboards: Er gab keine
zeilengenauen Vorgaben, sondern die grobe Idee, prüfte den vorgeschlagenen Plan und ließ
Antigravity die Umsetzung übernehmen. Projektstruktur, Oberfläche, Logik und Tests entstanden
weitgehend selbstständig; eingegriffen wurde erst bei visuellen Details und Interaktionen.

Sein Fazit: Antigravity helfe nicht dabei, Arbeit zu *organisieren*, sondern sie
**abzuschließen**. Genau darin liegt der beschriebene Produktivitätsgewinn.

> **Hinweis zur Quelle:** Der Artikel ist ein Erfahrungsbericht, kein Test und kein Vergleich.
> Er nennt keine Nachteile, keine Kosten und keine Datenschutzaspekte. Das ist bei
> Tech-Publikationen üblich, heißt aber, dass die Einordnung unten *nicht* aus dem Artikel
> stammt.

---

## Kritische Einordnung

Antigravity ist ein gutes Werkzeug. Es ist gleichzeitig fast ein Musterbeispiel für die
Abhängigkeiten, die dieser Kurs sichtbar machen will.

### 1. Dein Code verlässt deinen Rechner

Die Modelle laufen nicht lokal, sondern in Googles Rechenzentren. Damit die Agenten arbeiten
können, brauchen sie Kontext, und der besteht aus deinem Quellcode, deiner Projektstruktur,
oft auch aus Konfigurationsdateien. Bei einem Hobbyprojekt ist das unkritisch. Bei Code eines
Arbeitgebers, eines Vereins oder mit Kundendaten ist es eine Weitergabe an Dritte, die
arbeitsrechtlich und datenschutzrechtlich geprüft werden muss.

Besonders heikel: Zugangsdaten, die versehentlich im Projekt liegen. Siehe
[Geheimnisse schützen](../server-hardening/04-geheimnisse-schuetzen.md).

### 2. Der Cloud Act gilt auch hier

Google ist ein US-Unternehmen. Damit greift derselbe Mechanismus, den das Kursmodul
[Digitale Unabhängigkeit](../../kurs/01-einstieg-open-source/01-digital-independence.md)
für Gmail und Google Drive beschreibt: Der **Cloud Act** verpflichtet US-Unternehmen, US-Behörden
Zugriff auf gespeicherte Daten zu gewähren, unabhängig vom Serverstandort. Wer Nextcloud
selbst hostet, um genau das zu vermeiden, und gleichzeitig seinen Code durch Antigravity
schickt, hat die Abhängigkeit nur verschoben.

### 3. Closed Source, nicht self-hostbar

Antigravity ist proprietär. Du kannst weder prüfen, was der Client tatsächlich überträgt, noch
das Werkzeug auf eigener Infrastruktur betreiben. Beides sind Anforderungen, die wir in diesem
Kurs an andere Software selbstverständlich stellen.

### 4. Abschaltrisiko

Google hat Produkte dutzende Male eingestellt. Antigravity startete ausdrücklich als
*experimentelles* Produkt. Ein Werkzeug, um das herum du deinen Arbeitsablauf baust, kann
verschwinden oder kostenpflichtig werden — ein bekanntes Muster (siehe
[Lock-in](../glossar.md#lock-in--vendor-lock-in)).

### 5. Der stille Preis: Kompetenz

Dieser Punkt ist der unbequemste, weil er nichts mit Google zu tun hat. Das Kursmodul zu
digitaler Unabhängigkeit formuliert es so: Offene Software allein reicht nicht, es braucht
**die Kompetenz, sie eigenständig zu beurteilen, einzusetzen und weiterzuentwickeln**.

Wer Aufgaben vollständig delegiert, baut diese Kompetenz nicht auf. Das ist kein Argument
gegen KI-Werkzeuge, aber ein Argument dafür, generierten Code zu *verstehen*, bevor man ihn
übernimmt. Souveränität über ein System, das man nicht mehr selbst erklären kann, ist keine
Souveränität.

---

## Alternativen mit mehr Kontrolle

Wenn du KI-Unterstützung willst, ohne den Code aus der Hand zu geben, gibt es Abstufungen:

| Ansatz | Beispiele | Unabhängigkeitsgewinn |
|---|---|---|
| Lokale Modelle | [Ollama](https://ollama.com/), [llama.cpp](https://github.com/ggml-org/llama.cpp) | Hoch: nichts verlässt den Rechner |
| Offene Editor-Integration | [Continue](https://www.continue.dev/), [Aider](https://aider.chat/) | Hoch: Open Source, freie Wahl des Modells |
| Self-hosted Assistent | [Tabby](https://www.tabbyml.com/) | Hoch: eigener Server, eigene Kontrolle |
| Cloud-Modell, offener Client | Continue + beliebige API | Mittel: Client prüfbar, Modell extern |
| Vollintegrierte Cloud-IDE | Antigravity, Cursor, Copilot | Gering |

Zwei Einschränkungen, damit die Tabelle nicht zu optimistisch wirkt:

- **Lokale Modelle brauchen Hardware.** Brauchbare Coding-Modelle wollen viel RAM und
  idealerweise eine GPU. Auf einem älteren Laptop ist das keine echte Option.
- **„Offene Gewichte" ist nicht dasselbe wie Open Source.** Viele frei herunterladbare Modelle
  stehen unter eigenen Lizenzen mit Nutzungseinschränkungen und erfüllen die
  Open-Source-Definition nicht. Siehe [Lizenzen](../../kurs/01-einstieg-open-source/03-lizenzen.md).

---

## Fazit

Der XDA-Artikel hat in dem recht, was er behauptet: Antigravity ist produktiv. Er ist nur
keine vollständige Entscheidungsgrundlage, weil er die Kosten nicht benennt.

Für diesen Kurs gilt die Leitfrage aus dem Modul zur digitalen Unabhängigkeit — das Ziel ist
nicht vollständige Autarkie, sondern **bewusste Abhängigkeit**. Übersetzt auf Antigravity:

- Für ein Wochenendprojekt ohne sensible Daten: eine legitime, bewusste Entscheidung.
- Für Code mit Kunden-, Vereins- oder Gesundheitsdaten: kritisch, ohne vorherige Prüfung eher nicht.
- Als dauerhafter Kern des eigenen Arbeitsablaufs: dann bitte mit dem Wissen, dass du
  Betriebsabhängigkeit von einem Anbieter aufbaust, den du weder prüfen noch ersetzen kannst.

Die falsche Reaktion wäre, das Werkzeug zu verteufeln. Die ebenso falsche wäre, es ohne diese
Fragen zu übernehmen.

---

## Quellen

- [Google Antigravity is my secret productivity weapon that everyone should know about](https://www.xda-developers.com/google-antigravity-secret-productivity-weapon-that-everyone-should-know/) – XDA Developers (Ausgangsquelle)
- [antigravity.google](https://antigravity.google/) – offizielle Produktseite
- [Digitale Unabhängigkeit](../../kurs/01-einstieg-open-source/01-digital-independence.md) – Kursmodul, Grundlage der Einordnung
