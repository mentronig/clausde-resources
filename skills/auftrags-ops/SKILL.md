---
name: auftrags-ops
description: Freelancer-Auftrags-Pipeline — Scoring, Scan, Tracker, Erstgespräch-Prep, Auslastung
user_invocable: true
args: mode
---

# auftrags-ops — Router

## Mode Routing

Bestimme den Mode aus `{{mode}}`:

| Input | Mode |
|-------|------|
| (leer / keine Argumente) | `discovery` — Menü anzeigen |
| `bewerten` oder eingefügter Ausschreibungstext | `bewerten` |
| `scan` | `scan` |
| `tracker` | `tracker` |
| `prep` | `prep` |
| `auslastung` | `auslastung` |

**Auto-Erkennung:** Wenn `{{mode}}` kein bekannter Sub-Command ist UND Ausschreibungstext enthält (Signalwörter: "Anforderungen", "Qualifikationen", "Aufgaben", "Einsatzort", "Stundensatz", "Projektbeschreibung", Firmenname + Rolle), führe `bewerten` aus.

---

## Discovery Mode (keine Argumente)

Zeige dieses Menü:

```
auftrags-ops — Freelancer-Pipeline

Verfügbare Commands:
  /auftrags-ops bewerten {text}   → Ausschreibung/Anfrage bewerten (A-F Scoring)
  /auftrags-ops scan              → E-Mail-Alerts parsen + WebSearch nach Ausschreibungen
  /auftrags-ops tracker           → Pipeline-Dashboard anzeigen/aktualisieren
  /auftrags-ops prep {auftrag}    → Erstgespräch-Vorbereitung
  /auftrags-ops auslastung        → Aktuelle Kapazität + geplante Aufträge

Oder füge eine Ausschreibung direkt ein — sie wird automatisch bewertet.
```

Zusätzlich: Lies `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Auftraege-Tracker.md` und zeige eine Kurzübersicht der letzten 5 Einträge.

---

## Kontext laden

### Vor JEDEM Mode:

1. Lies `references/_shared.md` (Scoring-Schema, Benchmarks)
2. Lies `references/_profile.md` (Rolands Kompaktprofil)
3. Lies den Vault Hot Cache: `C:/opt/Projects/obsidian-ws/Mentronig/00 Kontext/hot.md`

### Zusätzlich je Mode:

| Mode | Zusätzliche Dateien |
|------|---------------------|
| `bewerten` | `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Profile/Lebenslauf.md`, `references/_calibration.md` (frühere Lektionen für implizites Reasoning) |
| `scan` | `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Auftraege-Tracker.md` (Dedup), `references/_calibration.md` |
| `tracker` | `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Auftraege-Tracker.md` |
| `prep` | Vault-Kundenprofil unter `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Kunden/`, relevante `analyse/anfrage-analyse.md` |
| `auslastung` | `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Auftraege-Tracker.md`, Vault Hot Cache |

---

## Mode: bewerten

Bewerte eine Freelancer-Ausschreibung oder -Anfrage nach dem 6-Dimensionen-Scoring.

### Schritt 1 — Ausschreibung erfassen

Wenn der Input eine **URL** ist: Versuche den Inhalt mit WebFetch zu laden. Falls das fehlschlägt (Login-Wall), bitte Roland den Text manuell einzufügen.

Wenn der Input **Text** ist: Direkt verwenden.

### Schritt 2 — Kontext laden

1. Lies `references/_shared.md` und `references/_profile.md`
2. Lies `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Profile/Lebenslauf.md` für die Match-Analyse
3. Prüfe ob der Kunde/Vermittler im Vault bekannt ist:
   - Kunden: `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Kunden/`
   - Vermittler: `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Vermittler/`
   - Falls bekannt: Lies das Profil — bisherige Erfahrungen, akzeptierte Stundensätze, Lessons Learned fließen in die Bewertung ein

### Schritt 3 — Scoring durchführen

Bewerte nach den 6 Dimensionen aus `_shared.md`. Für jede Dimension:
1. Score (1–5) vergeben
2. Kurze Begründung (1–2 Sätze)
3. Gewichteter Beitrag berechnen

### Schritt 4 — Scoring-Output

```markdown
## Scoring: [Kunde] – [Rolle]

**Quelle:** [Vermittler/Plattform/Direkt]
**Datum:** [YYYY-MM-DD]

| # | Dimension | Score | Gewicht | Kommentar |
|---|-----------|-------|---------|-----------|
| 1 | Rollenpassung | X/5 | 25% | ... |
| 2 | Kommerz | X/5 | 20% | ... |
| 3 | Kundenfit | X/5 | 20% | ... |
| 4 | Logistik | X/5 | 15% | ... |
| 5 | Red Flags | X/5 | 10% | ... |
| 6 | Strategischer Wert | X/5 | 10% | ... |

**Global Score: X.X/5 → Note: [A-F]**

### Empfehlung
[Sofort bewerben / Bewerben + verhandeln / Nur bei Kapazität / Absagen]

### Stärken
- ...

### Lücken / Risiken
- ...

### Stundensatz-Einschätzung
[Analyse des genannten/geschätzten Stundensatzes vs. Benchmark]
```

### Schritt 5 — Tracker aktualisieren

Lies den aktuellen Auto-Reject-Schwellenwert aus `references/_calibration.md` (Standard: 3,8).

- **Score ≥ Schwellenwert:** Eintrag mit Status `Bewertet`
- **Score < Schwellenwert:** Eintrag mit Status `Abgelehnt` — kein weiterer Aufwand

Falls der Kunde+Rolle bereits existiert: bestehenden Eintrag aktualisieren, nicht duplizieren.

### Schritt 6 — Nächste Schritte empfehlen

Basierend auf dem Score:
- **Score ≥ 3,8 (B+/A):** "Soll ich `/anfrage` starten und das volle Profil erstellen?"
- **Score 3,8–4,4 (B):** "Solider Auftrag. Bewerben und Stundensatz durchsetzen?"
- **Score < 3,8:** "Auto-Reject (unter Schwellenwert 3,8). Im Tracker als Abgelehnt eingetragen."

---

## Mode: scan

Scannt den Gmail-Ordner `Auftraege/Scan` auf neue Projekt-Alerts und bewertet alle enthaltenen Ausschreibungen automatisch.

### Schritt 1 — Neue Mails abrufen

Suche in Gmail nach Mails im Label `Auftraege/Scan` die noch nicht archiviert sind:

```
label:Auftraege/Scan -label:Auftraege/Archiv
```

Verwende `mcp__gmail__search_emails` mit Query `label:Auftraege/Scan -label:Auftraege/Archiv`, maxResults 20.

Falls keine Mails: Ausgabe `Keine neuen Mails in Auftraege/Scan.` und beenden.

### Schritt 2 — Mails lesen und Projekte extrahieren

Für jede gefundene Mail: Lies den vollständigen Inhalt mit `mcp__gmail__read_email`.

Extrahiere alle enthaltenen Projektausschreibungen. Eine Mail kann mehrere Projekte enthalten (typisch bei Projektagent-Alerts). Erkenne Projekte an:
- Überschriften mit Rollenbezeichnung
- Blöcken mit Angaben zu Aufgaben, Anforderungen, Einsatzort, Stundensatz/Budget, Laufzeit
- Trennlinien oder nummerierten Einträgen zwischen einzelnen Projekten

Pro Projekt erfasse:
- **Titel/Rolle** (z.B. "Scrum Master (m/w/d)")
- **Kunde oder Endkunde** (falls erkennbar)
- **Vermittler/Quelle** (z.B. Freelance.de, Gulp, Freelancermap — aus Mail-Absender/Inhalt ableiten)
- **Stundensatz / Budget** (falls angegeben)
- **Laufzeit / Start** (falls angegeben)
- **Einsatzort / Remote-Anteil** (falls angegeben)
- **Kernanforderungen** (Stichworte)
- **Link zur Ausschreibung** (falls vorhanden)

### Schritt 3 — Dedup gegen Tracker

Lies `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Auftraege-Tracker.md`. Prüfe für jedes extrahierte Projekt: Ist eine Kombination aus ähnlicher Rolle + ähnlichem Kunden/Vermittler bereits eingetragen?
- Falls ja und Status ≠ `Neu`/`Bewertet`: Projekt überspringen, kurze Notiz ausgeben ("Bereits in Pipeline: ...")
- Falls nein oder Status `Neu`: Weiter zu Schritt 4

### Schritt 4 — Automatisches Scoring

Führe für jedes neue Projekt das vollständige Scoring aus (identisch mit `bewerten`-Mode, Schritte 2–4). Nutze die extrahierten Daten als Input.

Bei fehlenden Informationen (z.B. kein Stundensatz angegeben):
- Kommerz: Score 2 vergeben + Kommentar "Stundensatz unbekannt — Rückfrage nötig"
- Logistik: Falls Einsatzort fehlt, Score 3 (neutral) + Kommentar "Einsatzort unbekannt"

### Schritt 5 — Tracker aktualisieren

Lies den Auto-Reject-Schwellenwert aus `references/_calibration.md` (Standard: 3,8).

- **Score ≥ Schwellenwert:** Status `Bewertet`
- **Score < Schwellenwert:** Status `Abgelehnt` — kein weiterer Aufwand, kein Vorschlag zur Bewerbung

### Schritt 6 — Mails archivieren

Für jede verarbeitete Mail: Füge das Label `Auftraege/Archiv` hinzu (erstelle es falls nicht vorhanden mit `mcp__gmail__get_or_create_label`), entferne das Label `Auftraege/Scan` mit `mcp__gmail__modify_email`.

### Schritt 7 — Scan-Zusammenfassung ausgeben

```
Scan abgeschlossen — [Datum]

Mails verarbeitet: X
Projekte gefunden: Y
  Neu bewertet: Z
  Bereits in Pipeline: N (übersprungen)

## Bewertungen

[Für jedes neue Projekt: Kompakt-Scoring-Output aus bewerten-Mode]

---
Tracker aktualisiert. Soll ich für eines der A/B-Projekte direkt /anfrage starten?
```

---

## Mode: tracker

Lies `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Mentronig/Kundenbetreuung/Auftraege-Tracker.md` und zeige eine formatierte Übersicht:

1. **Aktive Pipeline:** Alle Einträge mit Status ≠ Gewonnen/Verloren/Abgelehnt
2. **Statistik:** Anzahl pro Status, Durchschnittsscore, Score-Verteilung
3. **Letzte Änderungen:** Die 3 neuesten Einträge

Falls Roland einen Status aktualisieren möchte ("KfW auf Gewonnen setzen"), die Änderung in `Auftraege-Tracker.md` durchführen und ggf. Vault-Rückfluss vorschlagen.

### Reflexions-Trigger (Kalibrierungs-Loop)

Wenn ein Status auf einen **Terminal-Wert** gesetzt wird (`Gewonnen`, `Verloren`, `Abgelehnt`):

**Schritt 1 — Reflexion durchführen**

Vergleiche den ursprünglichen Score mit dem tatsächlichen Outcome:

```
Reflexion: [Kunde / Rolle]
Score war:    [Note] ([X.X]/5)
Outcome:      [Gewonnen / Verloren / Abgelehnt]
Abweichung:   [Überraschend? Erwartbar? Welche Dimension hat versagt?]
Ursache:      [Stundensatz / Profil-Lücke / Timing / Marktbedingung / ...]
Lektion:      [Konkrete Faustregel für ähnliche zukünftige Fälle]
```

**Schritt 2 — Eintrag in `_calibration.md` schreiben**

Füge den Reflexions-Eintrag unter `## Einträge` in `references/_calibration.md` ein.

**Schritt 3 — Weekly Review prüfen (Ebene 1 + Ebene 2)**

Prüfe ob seit dem letzten Review neue Einträge vorhanden sind (Scoring-Einträge UND Prozess-Reflexions-Einträge in `_calibration.md`). Falls ja und vergangene Woche nicht reviewed:

Review-Output umfasst beide Ebenen:
- **Ebene 1 (Scoring):** Score vs. Outcome — Gewichte und Schwellenwert anpassen?
- **Ebene 2 (Prozess):** Offene Prozess-Reflexionen mit Status ⏳ — konkrete SKILL.md / _shared.md Änderungen vorschlagen

Erst nach Rolands Bestätigung werden Änderungen an `_shared.md` oder `SKILL.md` vorgenommen.

Der Review enthält auch eine Einschätzung ob der Auto-Reject-Schwellenwert (aktuell: 3,8) angepasst werden sollte — nach oben bei voller Pipeline, nach unten bei Auslastungslücke.

---

## Mode: prep

**Status:** Phase 3 — noch nicht implementiert. Zeige:

```
Prep-Mode ist in Phase 3 geplant.

Workaround: Lies die anfrage-analyse.md und das Kundenprofil im Vault
für Erstgespräch-Vorbereitung.
```

---

## Mode: auslastung

**Status:** Phase 4 — noch nicht implementiert. Zeige:

```
Auslastungs-Mode ist in Phase 4 geplant.

Workaround: Prüfe den Tracker (/auftrags-ops tracker) und den Hot Cache
für den aktuellen Stand.
```

---

## Wichtige Regeln

- **Sprache:** Deutsch
- **Umlaute:** ö, ä, ü, ß korrekt ausschreiben
- **Keine Annahmen** über Rolands Erfahrungen — im Zweifel nachfragen
- **Scoring ist reproduzierbar** — gleiche Eingabe → gleicher Score
- **Vault-Daten nutzen** — Bestandskunden-Bonus, Vermittler-Erfahrungen, Preishistorie
- **Tracker immer aktuell halten** — jede Bewertung wird eingetragen
- **Nie Profildaten erfinden** — nur was in Lebenslauf.md und Kurzprofil.md steht
