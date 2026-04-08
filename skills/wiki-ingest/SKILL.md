---
name: wiki-ingest
description: Verarbeitet Rohdaten (Notizen, Artikel, Transkripte) aus der Inbox in vernetzte Wiki-Seiten nach dem Karpathy LLM-Wiki-Ansatz
user_invocable: true
---

# Wiki Ingest

Du verarbeitest Rohdaten aus der Inbox und erstellst daraus vernetzte Wiki-Seiten in `09 Wiki/`.

## Schritt 1: Rohdaten lesen

Lies die Dateien aus `01 Inbox/onenote-import/` (oder einem vom User angegebenen Pfad).

Für jeden gefundenen Inhalt:
- **Markdown (.md):** Direkt lesen
- **DOCX (.docx):** Mit `python-docx` oder `pandoc` in Markdown konvertieren. Falls keines installiert: User darauf hinweisen.
- **PDF (.pdf):** Mit dem Read-Tool lesen (unterstützt PDFs nativ)
- **Textdateien (.txt):** Direkt lesen

Erstelle eine Bestandsliste: Dateiname, Typ, ungefähre Länge, erkannte Sprache.

## Schritt 2: Inhalte analysieren

Für jede Quelldatei:
1. **Kernthemen identifizieren** — Was sind die Hauptkonzepte?
2. **Entitäten extrahieren** — Personen, Firmen, Tools, Technologien
3. **Querverbindungen erkennen** — Welche Konzepte hängen zusammen? Gibt es Verbindungen zu bestehenden Wiki-Seiten?
4. **Qualität bewerten** — Ist der Inhalt vollständig? Veraltet? Nur Stichworte?

## Schritt 3: Vorschau zeigen

Zeige dem User eine strukturierte Vorschau:

```
## Ingest-Vorschau

### Quelle: <Dateiname>
- Sprache: Deutsch
- Umfang: ~500 Wörter
- Qualität: Vollständig / Stichworte / Gemischt

### Geplante Wiki-Seiten:
1. concepts/Cloud Computing.md — Kernkonzept, verlinkt mit [[AWS]], [[Azure]]
2. entities/Microsoft.md — Erwähnt als Cloud-Anbieter
3. sources/<Dateiname>.md — Quellenverweis

### Übersprungen:
- Screenshot auf Seite 3 (nicht lesbar)
- Abschnitt "TODO" (nur Stichworte ohne Kontext)

Soll ich fortfahren?
```

**Wichtig:** Nichts erstellen ohne Bestätigung des Users!

## Schritt 4: Wiki-Seiten erstellen

Nach Bestätigung, erstelle die Wiki-Seiten:

### Ordnerstruktur

Konzepte werden nach **Domäne** in Unterordner gruppiert (eine Ebene tief):

```
concepts/
├── Prompt Engineering/
├── Context Engineering/
├── Agile/
├── Lernen & Schulungen/
├── Produkt Management/
├── Design Thinking/
├── Tools/
├── Kunden/
└── <weitere Domänen nach Bedarf>/
```

Beim Ingest: Identifiziere die Domäne der Quellinhalte. Falls der Unterordner noch nicht existiert, erstelle ihn. Keine weitere Verschachtelung innerhalb der Domänen-Ordner.

### Format für Konzept-Seiten (`concepts/<Domäne>/`)

```markdown
---
tags: [wiki, concept, <thema>]
erstellt: <datum>
quellen: ["<quelldatei>"]
---

# <Konzeptname>

<Zusammenfassung in 2-3 Sätzen>

## Details

<Ausführlichere Beschreibung basierend auf der Quelle>

## Verbindungen

- [[Verwandtes Konzept 1]] — <kurze Erklärung der Beziehung>
- [[Verwandtes Konzept 2]] — <kurze Erklärung der Beziehung>

## Quellen

- [[sources/<Quelldatei>]]
```

### Format für Entitäten-Seiten (`entities/`)

```markdown
---
tags: [wiki, entity, <typ>]
erstellt: <datum>
typ: person | firma | tool
---

# <Name>

<Kurzbeschreibung>

## Kontext

<Warum ist diese Entität relevant? In welchem Zusammenhang taucht sie auf?>

## Verbindungen

- [[Konzept oder andere Entität]] — <Beziehung>
```

### Format für Quellen-Seiten (`sources/`)

```markdown
---
tags: [wiki, source]
erstellt: <datum>
original: <Dateiname der Quelldatei>
---

# Quelle: <Titel>

- **Ursprung:** <OneNote / Artikel / Transkript / etc.>
- **Verarbeitet am:** <Datum>

## Erstellte Wiki-Seiten

- [[concepts/Seite 1]]
- [[entities/Seite 2]]
```

## Schritt 5: Index und Log aktualisieren

### index.md

Füge die neuen Seiten in die passenden Kategorien ein. Halte die Listen alphabetisch sortiert.

### log.md

Füge eine neue Zeile in die Log-Tabelle ein:

```
| <Datum> | Ingest | <Quelldatei> | <Anzahl> Seiten erstellt |
```

## Schritt 6: Aufräumen

Frage den User: **"Soll ich die verarbeiteten Quelldateien aus der Inbox verschieben?"**

Falls ja: Verschiebe nach `06 Archiv/onenote-import/` (erstelle den Ordner falls nötig).

## Wichtige Regeln

- **Niemals Seiten erstellen ohne User-Bestätigung** (Vorschau in Schritt 3)
- **Bestehende Wiki-Seiten erweitern** statt duplizieren — wenn ein Konzept schon existiert, ergänze es
- **Veraltete/irrelevante Inhalte überspringen** — User fragen ob diese trotzdem aufgenommen werden sollen
- **Wikilinks verwenden** für alle Verknüpfungen
- **Umlaute korrekt** — ö, ä, ü, ß
- **Sprache:** Deutsch, sachlich-professionell
