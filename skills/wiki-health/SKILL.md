---
name: wiki-health
description: Analysiert das Wiki inhaltlich auf Lücken, Widersprüche, Vokabular-Inkonsistenz und Struktur-Schwächen — schlägt Artikel-Kandidaten und Verbesserungen vor
user_invocable: true
---

# Wiki Health Check

Du analysierst das Wiki unter `09 Wiki/` inhaltlich und strukturell-qualitativ — nicht strukturell-technisch (das macht `/wiki-lint`). Du liest alle Seiten, erkennst was fehlt oder inkonsistent ist, und schlägst Verbesserungen vor. **Nichts wird automatisch geändert.**

## Abgrenzung zu `/wiki-lint`

| `/wiki-lint` | `/wiki-health` |
|-------------|----------------|
| Strukturprüfung (Links, Frontmatter, Index) | Inhaltliche Qualitätsanalyse |
| Findet kaputte Verknüpfungen | Findet fehlende Konzepte und Widersprüche |
| Regelbasiert, deterministisch | LLM-Analyse, interpretativ |

Empfohlen: erst `/wiki-lint`, dann `/wiki-health`.

---

## Schritt 1: Wiki vollständig lesen

Lies alle `.md`-Dateien unter `09 Wiki/` rekursiv:
- `concepts/` — alle Konzept-Seiten
- `entities/` — alle Entitäten-Seiten
- `sources/` — alle Quellen-Seiten
- `index.md` — Inhaltsverzeichnis
- `log.md` — Änderungshistorie
- `CLAUDE.md` im Vault-Root — aktuelle Wiki-Regeln und Konventionen

Erstelle intern:
- Liste aller vorhandenen Konzepte und Entitäten (Name + Kernaussagen)
- Liste aller erwähnten Begriffe ohne eigene Seite
- Themencluster (Gruppen von inhaltlich verwandten Seiten)
- Begriffsregister (alle verwendeten Terme für dasselbe Konzept)

---

## Schritt 2: Analysen durchführen

### 2.1 Erwähnte aber nicht erklärte Konzepte

Finde Begriffe die in Seiten erwähnt oder verlinkt werden, aber keine eigene Wiki-Seite haben.

### 2.2 Thematische Lücken

Erkenne Themencluster und prüfe ob wichtige Teilbereiche fehlen. Beispiel: Das Wiki hat 8 Prompt-Engineering-Seiten — fehlt ein Überblicksartikel über Prompt-Strategie?

### 2.3 Ungenutzte inhaltliche Verbindungen

Finde Seiten die inhaltlich verwandt sind aber nicht verlinkt sind. Beispiel: `Sycophancy.md` und `Self-Critique und Reflection.md` behandeln verwandte Themen.

### 2.4 Einseitige Darstellungen

Erkenne Seiten wo ein Konzept nur aus einer Perspektive beschrieben ist — nur Vorteile ohne Nachteile, nur Theorie ohne Praxis, nur Anwendung ohne Grenzen.

### 2.5 Konzept-Drift-Erkennung

Finde Widersprüche zwischen Seiten: Dasselbe Konzept wird in verschiedenen Seiten unterschiedlich oder gegensätzlich beschrieben.

Vorgehen:
- Vergleiche Kernaussagen aller Seiten zum selben Themencluster
- Markiere Aussagen die sich widersprechen (nicht nur ergänzen)
- Unterscheide: echter Widerspruch vs. unterschiedliche Perspektiven (beide können korrekt sein)

Beispiel: `Coaching Prompts.md` beschreibt sie als Unterform von Few-Shot, `Few-Shot Prompting.md` behandelt sie als eigenständige Technik — Widerspruch oder vertretbare Sichtweisen?

### 2.6 Vokabular-Normalisierung

Finde Fälle wo dasselbe Konzept mit verschiedenen Begriffen bezeichnet wird.

Vorgehen:
- Erstelle ein internes Begriffsregister: welche Terme kommen wo vor?
- Erkenne Synonyme und Varianten (z.B. "LLM", "Sprachmodell", "KI-Modell", "Large Language Model")
- Prüfe ob es eine bevorzugte Schreibweise gibt oder ob alle Varianten gleichwertig verwendet werden

Ausgabe: Vorschlag für eine kanonische Schreibweise pro Konzept + Liste der Abweichungen.

### 2.7 Dead-End-Seiten (inhaltlich)

Finde Seiten von denen keine ausgehenden inhaltlichen Links ausgehen — Wissens-Sackgassen.

Hinweis: Technische tote Links prüft `/wiki-lint`. Hier geht es um Seiten die inhaltlich isoliert sind: Sie haben vielleicht gültige Links, aber keine inhaltlichen Verbindungen zu verwandten Konzepten.

### 2.8 Backlink-Dichte-Analyse

Identifiziere Hub-Seiten: Konzepte die von vielen anderen Seiten referenziert werden und daher besondere Bedeutung im Wiki haben.

- Seiten mit vielen eingehenden Links → Hub-Konzepte → sollten besonders tief und vollständig sein
- Prüfe ob Hub-Konzepte tatsächlich die Qualität haben die ihrer Rolle entspricht
- Seiten mit null eingehenden Links (außer index.md) → Kandidaten für Verwaiste-Seiten-Check in `/wiki-lint`

### 2.9 Kategorien-Evolution

Prüfe ob die aktuelle Ordnerstruktur noch passt oder ob neue Unterkategorien sinnvoll wären.

Vorgehen:
- Zähle Seiten pro Unterordner — ab ~10 Seiten in einem Cluster ist eine Unterkategorie sinnvoll
- Erkenne wiederkehrende Themen die noch keine eigene Kategorie haben
- Vergleiche mit den Kategorien in `CLAUDE.md` — stimmen sie noch überein?

Ausgabe: Vorschlag für neue Unterkategorien + welche Seiten dorthin verschoben werden würden. Keine automatischen Änderungen.

### 2.10 Quellenalterung (qualitativ)

Ergänzt die strukturelle Prüfung in `/wiki-lint` (2.5) um eine inhaltliche Einschätzung:

- Seiten zu schnelllebigen Themen (KI-Modelle, Tools, Frameworks) mit `erstellt`-Datum älter als 6 Monate
- Prüfe ob die Kernaussagen noch aktuell sein könnten — basierend auf dem Thema, nicht auf externem Wissen
- Beispiel: Eine Seite über "GPT-3" ist wahrscheinlich veraltet, eine über "Chain-of-Thought Prompting" weniger

---

## Schritt 3: Report erstellen

```
## Wiki Health Report — <Datum>

### Zusammenfassung
- Gelesene Seiten: <N>
- Artikel-Kandidaten: <N>
- Widersprüche: <N>
- Vokabular-Inkonsistenzen: <N>
- Dead-End-Seiten: <N>
- Hub-Konzepte: <N>
- Kategorien-Vorschläge: <N>

### Artikel-Kandidaten (priorisiert)

#### Hohe Priorität — oft erwähnt, fehlt komplett
1. **<Konzeptname>** (Typ: concept/entity)
   - Erwähnt in: <Liste der Seiten>
   - Warum wichtig: <1 Satz>
   - Mögliche Verlinkungen: <Liste>

#### Mittlere Priorität — thematische Lücke
1. **<Konzeptname>**
   - Gehört zu Cluster: <Themenbereich>
   - Warum es fehlt: <1 Satz>

#### Niedrige Priorität — Vertiefung bestehender Themen
1. **<Konzeptname>**
   - Vertieft: <vorhandene Seite>
   - Mehrwert: <1 Satz>

### Konzept-Drift — Widersprüche (<N>)
- `concepts/A.md` vs `concepts/B.md`
  - Widerspruch: <was genau widersprüchlich ist>
  - Empfehlung: <Zusammenführen / Perspektive klarstellen / Eine als kanonisch markieren>

### Vokabular-Inkonsistenzen (<N>)
- Konzept: <Name>
  - Gefundene Terme: <Liste aller verwendeten Varianten>
  - Empfehlung: <Kanonischer Begriff>
  - Betroffene Seiten: <Liste>

### Dead-End-Seiten (<N>)
- `concepts/XY.md` — keine ausgehenden inhaltlichen Links, verwandte Konzepte: <Vorschläge>

### Hub-Konzepte — Qualitätsprüfung (<N>)
- `concepts/XY.md` — <N> eingehende Links → Hub-Konzept
  - Qualität ausreichend: Ja / Nein
  - Falls nein: <Was fehlt>

### Kategorien-Vorschläge (<N>)
- Neue Kategorie: `concepts/<Neuer Ordner>/`
  - Begründung: <N> Seiten zu diesem Thema, wächst weiter
  - Betroffene Seiten: <Liste die verschoben würden>

### Quellenalterung — möglicherweise veraltet (<N>)
- `sources/XY.md` — erstellt <Datum>, Thema: <schnelllebig/stabil>
  - Empfehlung: Prüfen / Aktualisieren / Archivieren

### Einseitige Darstellungen (<N>)
- `concepts/XY.md` — <Was fehlt: Nachteile / Grenzen / Gegenargumente>

### Ungenutzte inhaltliche Verbindungen (<N>)
- `concepts/A.md` ↔ `concepts/B.md` — <Warum inhaltlich verwandt>
```

---

## Schritt 4: Änderungen umsetzen (auf Anweisung)

**Nur nach expliziter Bestätigung**, pro Problemtyp separat:

- **Neue Artikel**: User wählt Kandidaten → Seiten erstellen + verlinken + Index aktualisieren
- **Widersprüche beheben**: Seiten zusammenführen oder Perspektive klarstellen
- **Vokabular normalisieren**: Kanonischen Begriff in allen betroffenen Seiten einheitlich setzen
- **Dead-Ends schließen**: Ausgehende Links zu verwandten Konzepten ergänzen
- **Kategorien einführen**: Unterordner erstellen + Seiten verschieben + Index + CLAUDE.md Konventionen aktualisieren
- **CLAUDE.md-Changelog**: Jede Konventionsänderung mit Datum und Begründung in `CLAUDE.md` dokumentieren

### CLAUDE.md-Changelog-Konvention

Wenn Kategorien oder Wiki-Regeln geändert werden, wird ein Changelog-Eintrag in der Vault-CLAUDE.md ergänzt:

```markdown
## Wiki-Konventionen Changelog

| Datum | Änderung | Begründung |
|-------|---------|------------|
| YYYY-MM-DD | Neue Kategorie `concepts/Agile/` eingeführt | 12 Seiten zu Agile-Themen, klare Abgrenzung sinnvoll |
```

---

## Schritt 5: Log aktualisieren

```
| <Datum> | Health Check | — | <Zusammenfassung der Änderungen> |
```

---

## Wichtige Regeln

- **Niemals automatisch ändern** — immer erst Report zeigen, dann pro Punkt bestätigen lassen
- **Widerspruch ≠ Perspektive** — nicht jede Abweichung ist ein Fehler; unterschiedliche Sichtweisen können beide korrekt sein
- **Qualität vor Quantität** — lieber 3 gute Vorschläge als 20 marginale
- **Keine Spekulation** — nur Vorschläge die sich aus dem vorhandenen Wiki-Inhalt ableiten lassen
- **Umlaute korrekt** — ö, ä, ü, ß
- **Sprache:** Deutsch
