---
name: ki-radar
description: KI- und Claude Code Radar — täglich, wöchentlich oder themenbasiert. Multi-Source-Suche mit Synthese und Vault-Routing.
user_invocable: true
args: mode
---

# KI-Radar

Opinionierter Radar für Rolands KI-Monitoring. Drei Modi, mehrere Quellen parallel, Synthese statt Rohliste.

Referenz: `C:/opt/Projects/obsidian-ws/Mentronig/04 Ressourcen/KI-Radar/KI-Radar.md`

---

## Mode Routing

| Input | Mode |
|-------|------|
| (leer) | `täglich` |
| `täglich` | Tages-Scan: YouTube + Changelog + HN |
| `woche` | Wochen-Scan: GitHub Skills + MCP + HN Weekly + YouTube |
| `thema <query>` | Ad-hoc: alle Quellen parallel zu einem Thema |

---

## Suchpräferenzen (immer anwenden)

**YouTube:**
- Sortierung: `viewCount` (meistgesehen zuerst)
- Sprache: Englisch + Deutsch bevorzugt
- Filter danach: Hat diese Person es selbst gebaut? Kein Clickbait, keine Shorts unter 2 Minuten

**WebSearch:**
- Hacker News bevorzugt für Entwickler-Diskussionen
- Primärquellen bevorzugt (Anthropic direkt, Simon Willison, GitHub)

**Relevanz-Kategorien** (nur diese interessieren):
1. **Features** — Neue Claude Code / Anthropic Funktionen
2. **Skills & Tools** — Neue Skills, MCP-Server, Integrationen
3. **Use Cases** — Praktische Anwendungen (Fokus: gebaut, nicht beschrieben)
4. **Strategisch** — KI-Trends relevant für KI-Beratung + Scrum Master Positionierung

---

## Mode: täglich

**Ziel:** Was ist heute in der Claude Code / KI-Welt passiert? 5 Minuten.

### Schritt 1 — Parallel suchen

Führe diese drei Suchen gleichzeitig aus:

**A) YouTube — heutige Videos**
```
query: "Claude Code"
publishedAfter: [heutiges Datum]T00:00:00Z
order: viewCount
maxResults: 15
```

**B) Hacker News — Claude Code heute**
```
WebSearch: "Claude Code OR Anthropic site:news.ycombinator.com"
```

**C) Anthropic Changelog**
```
WebSearch: "Anthropic changelog release site:anthropic.com OR site:docs.anthropic.com"
```

### Schritt 2 — Filtern

Aus allen Ergebnissen: nur behalten was in die 4 Relevanz-Kategorien fällt.

Ausschließen:
- Titel mit "kills", "destroys", "you won't believe", reine Hype-Signale
- Videos unter 2 Minuten (Shorts)
- Nicht-englische / nicht-deutsche Inhalte außer bei klarem Mehrwert
- Duplikate (selbes Thema, verschiedene Quellen → beste Quelle behalten)

### Schritt 3 — Synthese

```
## KI-Radar — Täglich [DATUM]

### Was heute wirklich passiert ist
[2-4 Sätze: Das wichtigste in eigenen Worten — nicht Liste, sondern Einschätzung]

### Features
- [Titel] — [Quelle] — [1 Satz warum relevant]

### Skills & Tools
- [Titel] — [Quelle] — [1 Satz warum relevant]

### Use Cases
- [Titel] — [Quelle] — [1 Satz warum relevant]

### Strategisch
- [Titel] — [Quelle] — [1 Satz warum relevant]

### Rauschen (gefiltert)
[N] Ergebnisse ignoriert — [kurze Begründung: Clickbait / falsche Sprache / kein Mehrwert]
```

### Schritt 4 — Routing vorschlagen

Pro relevantem Ergebnis eine Empfehlung als Tabelle:

```
### KI-Radar — Routing-Vorschläge

| Ergebnis | Warum ausgewählt | Empfehlung | Link |
|----------|------------------|------------|------|
| [Titel] | [1 Satz: Warum relevant / warum gefiltert] | `/youtube-transkript` / `/wiki-ingest` / Daily Note / Nichts | [direkter Link] |
```

**Link-Regeln:**
- YouTube-Videos: `https://www.youtube.com/watch?v=[videoId]` — videoId aus der Suche verwenden
- Anthropic Features: direkte URL aus dem Changelog oder der News-Seite
- GitHub-Repos: direkte Repo-URL
- HN-Diskussionen: direkte HN-Thread-URL
- Artikel/Blogs: direkte Artikel-URL

**Empfehlungs-Logik:**
- `/youtube-transkript` → Videos mit hohem Lernwert (neue Technik, konkreter Workflow)
- `/wiki-ingest` mit Gate-Check → dauerhaft wertvolles Wissen (Konzept, Methode)
- **Daily Note** → operative Infos (neues Feature, Deadline, Tool-Version)
- **Nichts** → konsumieren, loslassen

### Schritt 5 — Scan in Daily Note persistieren

**Pflicht nach jedem Scan:** Den vollständigen Radar-Output (Synthese + Routing-Tabelle) in die Daily Note des aktuellen Tages schreiben.

**Pfad:** `C:/opt/Projects/obsidian-ws/Mentronig/05 Daily Notes/YYYY-MM-DD.md`

- Falls die Datei noch nicht existiert: Einfachen Header anlegen (`# YYYY-MM-DD`) und dann anhängen
- Falls sie bereits existiert: Als neuen Abschnitt `## KI-Radar — Täglich DATUM` anhängen
- Bereits erledigte Routing-Aktionen mit ✅ markieren

**Warum:** Routing-Vorschläge sind zeitgebunden und gehen beim Session-Ende verloren. Die Daily Note ist der einzige Ort, der garantiert erhalten bleibt.

---

## Mode: woche

**Ziel:** Strategischer Wochenüberblick. 20 Minuten. Freitags oder Montags.

### Schritt 1 — Parallel suchen

**A) YouTube — diese Woche**
```
query: "Claude Code"
publishedAfter: [Datum vor 7 Tagen]T00:00:00Z
order: viewCount
maxResults: 20
```

**B) GitHub — neue Claude Code Skills**
```
WebSearch: '"SKILL.md" "claude code" site:github.com'
+ WebSearch: '"awesome-claude-skills" OR "awesome-claude-code" site:github.com'
```

**C) MCP Server Repository — neue Einträge**
```
WebSearch: "new MCP server site:github.com/modelcontextprotocol"
```

**D) Hacker News — KI diese Woche**
```
WebSearch: "Claude Code OR Anthropic site:news.ycombinator.com after:[Datum vor 7 Tagen]"
```

**E) Simon Willison — diese Woche**
```
WebFetch: https://simonwillison.net/ (letzte Einträge prüfen)
```

### Schritt 2 — Filtern + Deduplizieren

Wie im Tages-Mode. Zusätzlich: Ergebnisse die bereits in früheren Tages-Scans erschienen sind markieren (auf Basis der Wiki-Log und Daily Notes der Woche — falls verfügbar).

### Schritt 3 — Synthese

```
## KI-Radar — Woche [KW XX / DATUM]

### Diese Woche auf einen Blick
[3-5 Sätze: Was war das Thema der Woche? Welche Entwicklung zieht sich durch?]

### Top-Entwicklungen
1. [Wichtigstes] — [Quelle + Link] — [Warum relevant]
2. ...

### Neue Skills & Tools (GitHub)
- [Repo-Name] — [Stars] — [Was es macht] — [Für Roland relevant weil...]

### Neue MCP-Server
- [Name] — [Was es integriert]

### Strategische Einschätzung
[1 Absatz: Was bedeutet das für Rolands Positionierung als KI-Berater + Scrum Master?]
```

### Schritt 4 — Routing vorschlagen

Wie täglich (Tabelle mit Links). Zusätzlich:
- **KI-Radar Seite aktualisieren** → neue Praktiker / Kanäle in `04 Ressourcen/KI-Radar/KI-Radar.md` ergänzen falls neue relevante Quelle entdeckt

---

## Mode: thema

**Ziel:** Deep-Dive zu einem spezifischen Thema. Ad-hoc, auf Anfrage.

Aufruf: `/ki-radar thema Claude Code hooks`

### Schritt 1 — Parallel suchen (alle Quellen gleichzeitig)

**A) YouTube**
```
query: [thema]
order: viewCount
maxResults: 10
```

**B) Hacker News**
```
WebSearch: "[thema] site:news.ycombinator.com"
```

**C) GitHub**
```
WebSearch: "[thema] site:github.com"
```

**D) Web allgemein**
```
WebSearch: "[thema] Claude Code 2026"
```

**E) Simon Willison**
```
WebSearch: "[thema] site:simonwillison.net"
```

### Schritt 2 — Synthese

```
## KI-Radar — Thema: [THEMA] / [DATUM]

### Was die Community dazu sagt
[Zusammenfassung: Was sind die 2-3 wichtigsten Erkenntnisse aus allen Quellen kombiniert?]

### Beste Quellen
1. [Titel] — [URL] — [Typ: Video / Artikel / Repo / Diskussion]
2. ...

### Offene Fragen / Widersprüche
[Was ist noch unklar? Wo gibt es unterschiedliche Meinungen?]

### Empfehlung
[Lohnt sich ein tieferer Dive? Welche Quelle zuerst?]
```

### Schritt 3 — Routing vorschlagen

Wie täglich (Tabelle mit Links).

---

## Wichtige Regeln

- **Parallel suchen** — alle Quellen eines Modus gleichzeitig aufrufen, nicht nacheinander
- **Synthese ist Pflicht** — keine rohe Ergebnisliste ohne Einschätzung
- **Relevanz-Filter immer anwenden** — lieber 3 gute als 15 mittelmäßige Treffer
- **Routing am Ende** — jeder Scan endet mit konkreten Vorschlägen was wohin geht
- **Sprache:** Deutsch
- **Umlaute:** ö, ä, ü, ß korrekt
