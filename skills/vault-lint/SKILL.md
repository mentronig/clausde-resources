---
name: vault-lint
description: Prüft den Obsidian Vault auf Hygiene — Frontmatter, verwaiste Dateien, Inbox-Stau, Daily Notes Lücken, tote Wikilinks und leere Ordner
user_invocable: true
---

# Vault Lint

Du prüfst den gesamten Obsidian Vault (außer `09 Wiki/` — dafür gibt es `/wiki-lint`) auf strukturelle Probleme und Hygiene. **Nichts wird automatisch gefixt.**

## Scope

Geprüft werden:
- `01 Inbox/`
- `02 Projekte/`
- `03 Bereiche/`
- `04 Ressourcen/`
- `05 Daily Notes/`
- `06 Archiv/` (nur Strukturprüfung)

**Nicht geprüft:** `00 Kontext/`, `07 Anhänge/`, `08 Speicher/`, `09 Wiki/` (eigener Lint)

## Schritt 1: Bestandsaufnahme

Lies alle `.md`-Dateien in den geprüften Verzeichnissen rekursiv. Erstelle eine interne Liste:
- Dateiname, Pfad, Unterordner
- Frontmatter-Felder
- Alle `[[Wikilinks]]`

## Schritt 2: Prüfungen durchführen

### 2.1 Inbox-Stau
Zähle alle Dateien in `01 Inbox/` (inkl. Unterordner).
- **Grün:** 0-5 Dateien
- **Gelb:** 6-15 Dateien
- **Rot:** >15 Dateien
- Liste die Dateien auf mit Alter (Tage seit letzter Änderung)
- Ignoriere: `Brain Dump.md` (ist eine permanente Datei)

### 2.2 Frontmatter-Konsistenz

**Projekte** (`02 Projekte/*.md`) — Pflichtfelder:
- `tags` (muss `projekt` enthalten)
- `status` (muss `aktiv`, `pausiert` oder `abgeschlossen` sein)
- `erstellt` (ISO-Datum)

**Bereiche** (`03 Bereiche/**/*.md`) — Pflichtfelder:
- `tags`

**Daily Notes** (`05 Daily Notes/*.md`) — Pflichtfelder:
- `tags` (muss `daily` enthalten)
- `date` (ISO-Datum, muss mit Dateiname übereinstimmen)

### 2.3 Tote Wikilinks (vault-weit)
Finde `[[Wikilinks]]` die auf nicht-existierende Dateien zeigen.
- Prüfe im gesamten Vault (alle Ordner)
- Ignoriere Links auf Dateien in `07 Anhänge/` (Bilder, PDFs — Obsidian löst die selbst auf)
- Ignoriere Links mit `|` Alias — prüfe nur den Ziel-Pfad

### 2.4 Verwaiste Dateien
Finde `.md`-Dateien die:
- Keine Tags haben UND
- Von keiner anderen Datei verlinkt werden
- Ausnahmen: `Brain Dump.md`, Daily Notes, `CLAUDE.md`

### 2.5 Daily Notes Lücken
Prüfe `05 Daily Notes/` auf fehlende Tage:
- Ermittle den Zeitraum vom ältesten bis zum neuesten Eintrag
- Liste fehlende Tage auf (nur Werktage Mo-Fr, Wochenenden ignorieren)
- Das ist ein **Hinweis**, kein Fehler

### 2.6 Leere Ordner
Finde Unterordner in `03 Bereiche/` und `04 Ressourcen/` die keine `.md`-Dateien enthalten.

### 2.7 Projekte ohne Aktivität
Finde Projektdateien in `02 Projekte/` mit `status: aktiv` deren letzte Änderung länger als 30 Tage zurückliegt.

## Schritt 3: Report erstellen

```
## Vault Lint Report — <Datum>

### Zusammenfassung
- Geprüfte Dateien: <N>
- Probleme: <N>
- Hinweise: <N>

### Inbox-Stau (<Ampel>)
- <N> Dateien in 01 Inbox/
- Älteste: <Dateiname> (<N> Tage alt)
- ...

### Frontmatter-Probleme (<N>)
- `02 Projekte/XY.md` — fehlt: `status`
- `05 Daily Notes/2026-01-15.md` — `date` stimmt nicht mit Dateiname überein
- ...

### Tote Wikilinks (<N>)
- `03 Bereiche/XY.md` Zeile 10: [[Nicht existierende Notiz]]
- ...

### Verwaiste Dateien (<N>)
- `04 Ressourcen/XY.md` — keine Tags, keine eingehenden Links
- ...

### Daily Notes Lücken (<N> fehlende Werktage)
- 2026-04-07 (Montag)
- ...

### Leere Ordner (<N>)
- `03 Bereiche/Leer/`
- ...

### Inaktive Projekte (<N>)
- `02 Projekte/Altes Projekt.md` — status: aktiv, letzte Änderung vor <N> Tagen
- ...
```

## Schritt 4: Auf Anweisung fixen

**Nur nach expliziter Bestätigung** durch den User, pro Problemtyp:

- **Frontmatter ergänzen:** Fehlende Felder einfügen
- **Tote Links:** Entfernen oder Zielseite erstellen
- **Verwaiste Dateien:** In Inbox verschieben oder Tags vergeben
- **Leere Ordner:** Entfernen (nach Bestätigung)
- **Inaktive Projekte:** Status auf `pausiert` setzen (nach Bestätigung)

**Nicht fixbar durch diesen Skill:**
- Inbox-Stau → manuell oder per Session-Routine einsortieren
- Daily Notes Lücken → optional nachholen

## Wichtige Regeln

- **Niemals automatisch fixen** — immer erst Report zeigen
- **Keine Dateien löschen** ohne User-Bestätigung
- **Keine Dateien verschieben** ohne User-Bestätigung
- **`06 Archiv/`** nur strukturell prüfen, keine inhaltlichen Änderungen vorschlagen
- **Umlaute korrekt** — ö, ä, ü, ß
- **Sprache:** Deutsch
