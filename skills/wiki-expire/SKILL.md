---
name: wiki-expire
description: Prüft das Wiki auf abgelaufene temporal-Seiten und project-Seiten abgeschlossener Projekte — schlägt Verlängern, Archivieren oder Löschen vor
user_invocable: true
---

# Wiki Expire

Du prüfst das Wiki auf Seiten deren `lifespan` abgelaufen ist oder deren Projekt-Kontext nicht mehr aktiv ist. **Nichts wird automatisch geändert.**

Typischer Aufruf: monatlich oder nach `/wiki-lint`-Report.

---

## Schritt 1: Alle lifespan-Tags erfassen

Lies alle `.md`-Dateien unter `09 Wiki/concepts/` und `09 Wiki/entities/` rekursiv.

Extrahiere aus dem Frontmatter:
- Dateiname, Pfad
- `lifespan`-Wert (falls vorhanden)
- `erstellt`-Datum

Kategorisiere:
- **evergreen** → keine Prüfung nötig, überspringen
- **temporal:YYYY-MM** → Ablaufdatum auslesen, mit heute vergleichen
- **project:<name>** → Projektstatus prüfen (siehe Schritt 2)
- **kein lifespan** → als Gate-Bypass markieren (Hinweis: besser durch `/wiki-lint` behandeln)

---

## Schritt 2: Projektstatus prüfen (für `project:`-Tags)

Für jede Seite mit `lifespan: project:<name>`:

Prüfe den Status des Projekts in `C:/opt/Projects/obsidian-ws/Mentronig/02 Projekte/<name>.md`:
- `status: aktiv` → Seite noch relevant, überspringen
- `status: abgeschlossen` oder `status: pausiert` → Seite als abgelaufen markieren
- Projektdatei nicht gefunden → als unklar markieren, Roland entscheiden lassen

---

## Schritt 3: Report erstellen

```
## Wiki Expire Report — <Datum>

### Zusammenfassung
- Geprüfte Seiten: <N>
- Abgelaufen (temporal): <N>
- Projekt abgeschlossen: <N>
- Unklar (Projekt nicht gefunden): <N>
- Gate-Bypässe (kein lifespan): <N> → besser: /wiki-lint laufen lassen

### Abgelaufen — temporal (<N>)

| Seite | Ablaufdatum | Erstellt | Empfehlung |
|-------|------------|---------|------------|
| concepts/XY.md | 2026-03 | 2025-09 | Prüfen — 7 Monate alt |

Optionen pro Seite: **Verlängern** (neues Ablaufdatum) / **Archivieren** / **Löschen**

### Abgelaufen — Projekt abgeschlossen (<N>)

| Seite | Projekt | Projektstatus | Empfehlung |
|-------|---------|--------------|------------|
| concepts/XY.md | auftrags-ops | abgeschlossen | Auf evergreen upgraden oder löschen |

Optionen: **Auf evergreen upgraden** (falls dauerhaft wertvoll) / **Archivieren** / **Löschen**

### Unklar — Projekt nicht gefunden (<N>)
- `concepts/XY.md` — lifespan: project:unbekannt — Projektdatei fehlt → Roland entscheiden lassen
```

---

## Schritt 4: Änderungen durchführen (auf Anweisung)

**Nur nach expliziter Bestätigung**, pro Seite oder pro Batch:

**Verlängern:**
- `lifespan`-Wert im Frontmatter aktualisieren (z.B. `temporal:2026-10` → `temporal:2027-04`)

**Auf evergreen upgraden:**
- `lifespan: temporal:YYYY-MM` → `lifespan: evergreen`

**Archivieren:**
- Seite nach `09 Wiki/archiv/<Unterordner>/` verschieben
- Alle eingehenden Links auf diese Seite im Wiki auf `[[archiv/...]]` aktualisieren
- Index-Eintrag entfernen

**Löschen:**
- Seite entfernen
- Alle eingehenden Links entfernen oder ersetzen
- Index-Eintrag entfernen
- Log-Eintrag schreiben: `| <Datum> | Expire | <Seite> | Gelöscht — lifespan abgelaufen |`

**Nach allen Änderungen:** `09 Wiki/log.md` aktualisieren.

---

## Wichtige Regeln

- **Niemals automatisch löschen oder verschieben** — immer erst Report zeigen
- **Evergreen-Seiten werden nie von diesem Skill angefasst**
- **Löschen nur nach expliziter Bestätigung pro Seite** — kein Batch-Delete ohne Einzelfreigabe
- **Gate-Bypässe (kein lifespan) werden nur gemeldet**, nicht behandelt — das ist Aufgabe von `/wiki-lint`
- **Umlaute korrekt** — ö, ä, ü, ß
- **Sprache:** Deutsch
