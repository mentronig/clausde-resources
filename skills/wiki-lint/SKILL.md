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
- Konzepte: `tags` (muss `wiki` und `concept` enthalten), `erstellt`, `quellen`, `lifespan`
- Entitäten: `tags` (muss `wiki` und `entity` enthalten), `erstellt`, `typ`, `lifespan`
- Quellen: `tags` (muss `wiki` und `source` enthalten), `erstellt`, `original`

**Lifespan-Werte:** Gültig sind `evergreen`, `temporal:YYYY-MM`, `project:<name>`. Fehlendes oder ungültiges `lifespan`-Feld ist ein **Fehler** (nicht nur Hinweis) — die Seite wurde vermutlich direkt abgelegt ohne Gate-Check.

### 2.5 Veraltete Inhalte
Finde Seiten deren `erstellt`-Datum älter als 6 Monate ist — als Hinweis, nicht als Fehler.

### 2.6 Fehlende Rück-Verlinkungen
Wenn Seite A auf Seite B verlinkt, sollte Seite B idealerweise auch auf A verweisen. Finde einseitige Links.

### 2.7 Dead-End-Seiten (strukturell)
Finde Seiten die **keine ausgehenden Wikilinks** haben.
- Ausnahmen: `sources/`-Seiten (die verweisen oft nur auf die Quelldatei)
- Nicht: Seiten die Links haben die zufällig kaputt sind (das deckt 2.1 ab)
- Hinweis: Seiten ohne ausgehende Links sind strukturelle Sackgassen — inhaltliche Ursachen analysiert `/wiki-health`

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

### Gate-Bypässe — kein lifespan-Tag (<N>) ⚠️
- `concepts/XY.md` — kein `lifespan`-Feld — vermutlich direkt abgelegt, Gate-Check ausstehend
- ...
  → Empfehlung: Gate-Check nachträglich durchführen oder `lifespan: evergreen` setzen falls bewusst permanent

### Fehlende Rück-Verlinkungen (<N>)
- `concepts/A.md` → `concepts/B.md` (B verlinkt nicht zurück)
- ...

### Dead-End-Seiten (<N>)
- `concepts/XY.md` — keine ausgehenden Wikilinks
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
- Dead-End-Seiten: ausgehende Links zu verwandten Konzepten vorschlagen (inhaltliche Auswahl durch `/wiki-health`)

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
