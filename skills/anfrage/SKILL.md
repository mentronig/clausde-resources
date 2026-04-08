---
name: anfrage
description: Startet den Anfragen-Workflow für Kundenanfragen aus der Inbox
user_invocable: true
---

# Anfragen-Workflow

Du bearbeitest eine neue Kundenanfrage aus der `inbox/`. Folge diesem Workflow Schritt für Schritt.

## Schritt 0: Kontext aus dem Vault laden

1. Lies den Hot Cache: `C:/opt/Projects/obsidian-ws/Mentronig/00 Kontext/hot.md`
2. Prüfe ob für den Kunden bereits ein Profil existiert unter `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Kundenbetreuung/Kunden/`
3. Falls ja: Lies das Kundenprofil — die Auftragshistorie enthält Erkenntnisse aus früheren Anfragen (akzeptierte Stundensätze, bewährte Profil-Anpassungen, bekannte Lücken)
4. Prüfe ob für den Vermittler ein Profil existiert unter `C:/opt/Projects/obsidian-ws/Mentronig/03 Bereiche/Kundenbetreuung/Vermittler/`
5. Falls ja: Lies das Vermittlerprofil — enthält Template-Format, Konditionen, Erfahrungswerte

## Schritt 1: Inbox sichten und Auftrag anlegen

1. Lies den Inhalt von `inbox/` und identifiziere die Anfrage-Dokumente
2. Frage den User nach dem Vermittler (falls nicht erkennbar) und dem Kunden/Projekt
3. Lege das Auftragsverzeichnis an:
   ```
   auftraege/YYYY-MM_kunde-projekt/
   ├── anfrage/
   ├── angebot/
   └── analyse/
   ```
4. Verschiebe die Dateien aus `inbox/` nach `anfrage/`
5. **Text-Spiegel erstellen:** Für jedes Binärdokument (PDF, DOCX, PPTX) eine kompakte `.md`-Version im selben Verzeichnis erstellen. Ab hier nur noch die .md-Version lesen.

## Schritt 2: Anfrage analysieren

1. Lies die Anfrage (nur die .md-Version!)
2. Erstelle `analyse/anfrage-analyse.md` mit:
   - Zusammenfassung (Kunde, Vermittler, Rolle, Projekt, Zeitraum, Ort, Umfang)
   - Fristen (Angebotsfrist, Bindefrist, Projektstart)
   - Geforderte Qualifikationen (Muss / Kann)
   - Bewertungskriterien
   - Passungsgrad (Stärken, Lücken, Empfehlung)
3. **Bei Annahmen über Rolands Qualifikationen → nachfragen, nicht raten**

## Schritt 3: Kurzprofil erstellen

1. Lies `profile/Lebenslauf.md` (vollständige Projekthistorie) und identifiziere die für die Anfrage relevanten Projekte und Erfahrungen
2. Prüfe ob ein Template für den Vermittler existiert unter `profile/vorlagen/{vermittler}/`
   - Ja → Template-Format verwenden (z.B. meta-system → pptx)
   - Nein → Standard-Kurzprofil basierend auf `profile/Kurzprofil.md`
3. Profil auf Basis von Lebenslauf + Analyse (Schritt 2) zuschneiden:
   - Relevante Projekte aus dem Lebenslauf auswählen und hervorheben
   - Schlüsselbegriffe der Ausschreibung spiegeln
   - Irrelevante Details kürzen
4. Ablage in `angebot/` + .md-Spiegel erstellen
5. **Bei Annahmen → nachfragen**

**Quellen für die Profilerstellung:**
- `profile/Lebenslauf.md` – Vollständige Projekthistorie (Primärquelle)
- `profile/Kurzprofil.md` – Bestehendes Kurzprofil als Formatvorlage

## Schritt 4: Profil-Anfrage-Abgleich

1. Kurzprofil kritisch gegen Anforderungen prüfen
2. Erstelle `analyse/profil-abgleich.md` mit:
   - Checkliste: Jede Anforderung → ✅ Erfüllt / ⚠️ Teilweise / ❌ Nicht erfüllt
   - Stärken (wo Roland die Anforderungen übertrifft)
   - Lücken (was fehlt oder schwach formuliert ist)
   - Optimierungsvorschläge (konkrete Textänderungen)
3. Optimiertes Profil dem User zur Freigabe vorlegen
4. **Bei Annahmen → nachfragen**

## Wichtige Regeln

- **Sprache:** Deutsch
- **Umlaute:** ö, ä, ü, ß korrekt ausschreiben
- **Tonalität:** Sachlich-professionell, kein Marketing-Blabla
- **Binärdokumente nie direkt lesen** – immer .md-Spiegel verwenden
- **Keine Annahmen** über Rolands Erfahrungen – im Zweifel nachfragen
- **Quellen:** `profile/Lebenslauf.md` (vollständig) und `profile/Kurzprofil.md` (Kurzform)
