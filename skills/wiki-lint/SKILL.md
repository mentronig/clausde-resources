---
name: wiki-lint
description: Prüft das Wiki (09 Wiki/) auf Konsistenz, verwaiste Seiten, tote Links und Lücken — erstellt einen Report zur Bestätigung
user_invocable: true
---

# Wiki Lint

Du prüfst das Wiki unter `09 Wiki/` auf strukturelle und inhaltliche Probleme und erstellst einen Report. **Nichts wird automatisch gefixt.**

## Schritt 1: Bestandsaufnahme

Lies alle `.md`-Dateien unter `09 Wiki/` rekursiv. Erstelle eine interne Liste:
- Dateiname, Pfad, Unterordner (`concepts/`, `entities/`, `sources/`, `analysis/`)
- Frontmatter-Felder (tags, erstellt, quellen)
- Alle `[[Wikilinks]]` in jeder Datei

## Schritt 2: Prüfungen durchführen

### 2.1 Tote Wikilinks
Finde alle `[[Wikilinks]]` die auf nicht-existierende Dateien zeigen.
- Prüfe sowohl mit als auch ohne Unterordner-Pfad (z.B. `[[Prompt Engineering]]` und `[[concepts/Prompt Engineering/Prompt Engineering]]`)
- Ignoriere Links die auf Dateien außerhalb von `09 Wiki/` zeigen (z.B. `[[00 Kontext/Über mich]]`)

### 2.2 Verwaiste Seiten
Finde Wiki-Seiten die von **keiner** anderen Seite verlinkt werden.
- `index.md` und `log.md` sind Ausnahmen (müssen nicht verlinkt sein)
- Quellen-Seiten (`sources/`) müssen mindestens von einer Konzept- oder Entitäten-Seite referenziert werden

### 2.3 Index-Abweichungen
Vergleiche `index.md` mit den tatsächlich existierenden Dateien:
- Seiten die existieren aber nicht im Index stehen
- Einträge im Index die auf nicht-existierende Seiten zeigen

### 2.4 Frontmatter-Konsistenz
Prüfe ob alle Wiki-Seiten die erforderlichen Frontmatter-Felder haben:
- Konzepte: `tags` (muss `wiki` und `concept` enthalten), `erstellt`, `quellen`
- Entitäten: `tags` (muss `wiki` und `entity` enthalten), `erstellt`, `typ`
- Quellen: `tags` (muss `wiki` und `source` enthalten), `erstellt`, `original`

### 2.5 Veraltete Inhalte
Finde Seiten deren `erstellt`-Datum älter als 6 Monate ist — als Hinweis, nicht als Fehler.

### 2.6 Fehlende Rück-Verlinkungen
Wenn Seite A auf Seite B verlinkt, sollte Seite B idealerweise auch auf A verweisen. Finde einseitige Links.

## Schritt 3: Report erstellen

Zeige dem User einen strukturierten Report:

```
## Wiki Lint Report — <Datum>

### Zusammenfassung
- Geprüfte Seiten: <N>
- Probleme gefunden: <N>
- Hinweise: <N>

### Tote Wikilinks (<N>)
- `concepts/XY.md` Zeile 15: [[Nicht existierendes Konzept]]
- ...

### Verwaiste Seiten (<N>)
- `entities/Tool.md` — wird von keiner Seite referenziert
- ...

### Index-Abweichungen (<N>)
- FEHLT IM INDEX: `concepts/Neues Konzept.md`
- TOTER INDEX-EINTRAG: [[concepts/Gelöschte Seite]]
- ...

### Frontmatter-Probleme (<N>)
- `concepts/XY.md` — fehlt: `quellen`
- ...

### Fehlende Rück-Verlinkungen (<N>)
- `concepts/A.md` → `concepts/B.md` (B verlinkt nicht zurück)
- ...

### Hinweise (nicht kritisch)
- `sources/Alt.md` — erstellt vor 8 Monaten, evtl. prüfen
- ...
```

## Schritt 4: Auf Anweisung fixen

**Nur nach expliziter Bestätigung** durch den User:
- Tote Links entfernen oder Zielseiten erstellen
- Verwaiste Seiten verlinken oder entfernen
- Index aktualisieren
- Frontmatter ergänzen
- Rück-Verlinkungen einfügen

Frage bei jedem Problemtyp separat: "Soll ich das beheben?"

## Schritt 5: Log aktualisieren

Nach Fixes einen Eintrag in `09 Wiki/log.md` hinzufügen:

```
| <Datum> | Lint | — | <N> Probleme behoben |
```

## Wichtige Regeln

- **Niemals automatisch fixen** — immer erst Report zeigen
- **Keine Seiten löschen** ohne User-Bestätigung
- **Umlaute korrekt** — ö, ä, ü, ß
- **Sprache:** Deutsch
