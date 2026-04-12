# Auftrags-Ops — Shared Context

## Scoring-Schema

### 6 Dimensionen

| # | Dimension | Gewicht | Skala | Beschreibung |
|---|-----------|---------|-------|-------------|
| 1 | **Rollenpassung** | 25% | 1–5 | Wie gut passen die Anforderungen zu Rolands Profil? |
| 2 | **Kommerz** | 20% | 1–5 | Stundensatz, Projektdauer, Volumen |
| 3 | **Kundenfit** | 20% | 1–5 | Branche, Unternehmenskultur, Bestandskunde |
| 4 | **Logistik** | 15% | 1–5 | Reiseaufwand, Remote-Anteil, Flexibilität |
| 5 | **Red Flags** | 10% | 1–5 | Warnzeichen (5 = keine Red Flags, 1 = schwere Bedenken) |
| 6 | **Strategischer Wert** | 10% | 1–5 | Referenzwert, Lernpotenzial, Türöffner |

### Berechnung

```
Global Score = (Rollenpassung × 0.25) + (Kommerz × 0.20) + (Kundenfit × 0.20) 
             + (Logistik × 0.15) + (Red Flags × 0.10) + (Strategischer Wert × 0.10)
```

### Noten-Schema

| Note | Score | Bedeutung | Empfohlene Aktion |
|------|-------|-----------|-------------------|
| A | 4.5–5.0 | Traumauftrag | Sofort bewerben, Priorität 1 |
| B+ | 4.0–4.4 | Sehr gut | Bewerben, Stundensatz durchsetzen |
| B | 3.5–3.9 | Gut | Bewerben, ggf. verhandeln |
| C | 2.5–3.4 | Solide | Bewerben wenn Kapazität frei |
| D | 1.5–2.4 | Mäßig | Nur bei Auslastungslücke |
| F | 1.0–1.4 | Passt nicht | Absagen |

---

## Scoring-Leitfaden pro Dimension

### 1. Rollenpassung (25%)

| Score | Kriterien |
|-------|-----------|
| 5 | Alle Muss-Anforderungen erfüllt + mehrere Kann-Anforderungen. Rolle liegt im Kernkompetenzbereich. |
| 4 | Alle Muss-Anforderungen erfüllt, einzelne Kann-Anforderungen offen. |
| 3 | Die meisten Muss-Anforderungen erfüllt, 1–2 Lücken mit Mitigationsstrategie. |
| 2 | Mehrere Muss-Anforderungen nicht erfüllt, Lücken schwer zu überbrücken. |
| 1 | Grundlegende Mismatch — falsche Rolle oder fehlende Kernkompetenz. |

**Rollentypen und Passung:**
- **Scrum Master / Agile Coach:** Kernrolle, 17+ Jahre Erfahrung → Basis-Score 4
- **KI-Berater / AI Engineer:** Starke Zweitrolle, KfW-Referenz → Basis-Score 3–4
- **Mischrolle (SM + KI):** Rolands Alleinstellungsmerkmal → Basis-Score 5
- **Reine Entwicklerrolle:** Nicht Rolands Fokus → Basis-Score 1–2

### 2. Kommerz (20%) — Zweistufig

**Stufe 1 — Scan/Erstbewertung (Stundensatz unbekannt):**
Bei Erstanfragen über Plattformen ist der Stundensatz strukturell unbekannt — kein Abzug, kein Bonus. Kommerz bewertet nur Laufzeit und Volumen:

| Score | Kriterien |
|-------|-----------|
| 5 | Laufzeit >6 Monate, Vollzeit |
| 4 | Laufzeit 4–6 Monate, Vollzeit |
| 3 | Laufzeit 2–4 Monate oder Teilzeit |
| 2 | Laufzeit <2 Monate |
| 1 | Keine Angaben zu Laufzeit und Auslastung |

**Stufe 2 — Nach Vermittler-Kontakt (Stundensatz bekannt):**
Stundensatz fließt als Korrekturfaktor auf den Gesamtscore ein. Score im Tracker nach dem Gespräch aktualisieren — mit Vermerk "(nach Stundensatz)":

| Stundensatz | Korrekturfaktor |
|-------------|-----------------|
| ≥110 €/h | +0,30 |
| 100–109 €/h | +0,15 |
| 95–99 €/h | ±0,00 (Benchmark Remote) |
| 85–94 €/h | −0,15 |
| <85 €/h | −0,30 |

**Benchmark-Werte:**
- Vor Ort: 100 €/h (Standard)
- Remote: 95 €/h (Standard)
- SAFe SM Finanzsektor: 90–120 €/h (Markt)
- AI Engineer: 100–130 €/h (Markt)
- KfW-Referenz: 100 €/h (durchgesetzt über Meta-System)

**Modifikatoren:**
- Langfristig (>6 Monate): Leichte Reduktion akzeptabel
- Kurzfristig (<3 Monate): Aufschlag gerechtfertigt — im Gespräch durchsetzen
- Reisekosten separat abrechenbar: +0,10 auf Gesamtscore

### 3. Kundenfit (20%)

| Score | Kriterien |
|-------|-----------|
| 5 | Bestandskunde mit positiver Historie + bevorzugte Branche |
| 4 | Bevorzugte Branche (Finanzsektor, Automotive) + professionelle Strukturen |
| 3 | Bekannte Branche, neutraler Eindruck |
| 2 | Unbekannte Branche oder Signale für schwierige Zusammenarbeit |
| 1 | Branche mit bekannten Problemen oder negative Vermittler-Erfahrung |

**Bestandskunden-Bonus:** +1 auf Basis-Score (max. 5)
- KfW → bekannt, positive Historie
- Weitere Bestandskunden: Aus Vault-Kundenprofilen lesen

**Bevorzugte Branchen:** Finanzdienstleistungen, Automotive, Handel, Gesundheitswesen

### 4. Logistik (15%)

| Score | Kriterien |
|-------|-----------|
| 5 | 100% Remote oder Einsatzort <30 km von Florstadt |
| 4 | Hybrid mit ≤2 Tagen/Woche vor Ort im Rhein-Main-Gebiet ODER 100% Remote bundesweit |
| 3 | Überwiegend vor Ort Rhein-Main ODER Reise bundesweit (Hotel) bei gutem Stundensatz (≥100 €/h) |
| 2 | Reise bundesweit bei unbekanntem/niedrigem Stundensatz ODER Ausland mit Pendelweg |
| 1 | Dauerhafter Auslandsaufenthalt oder Umzug erforderlich |

**Standort:** Florstadt (Wetterau, ca. 40 km nördlich von Frankfurt)
**Reisen ist OK** wenn Stundensatz ≥100 €/h (oder CH/AT mit entsprechend höherem Satz).
**Reisekosten separat abrechenbar:** +0,5 auf Score.
**Ausland (nicht DACH):** Hard-KO außer bei außergewöhnlichem Gesamtpaket.

### 5. Red Flags (10%)

Score 5 = keine Red Flags. Abzüge für:

| Signal | Abzug |
|--------|-------|
| Body-Leasing-Charakter (reines Einbuchen ohne Projektbeschreibung) | -2 |
| Unrealistische Anforderungen (10 Zertifizierungen, 20 Jahre Erfahrung mit 5 Jahre alter Technologie) | -1 |
| Negativer Vermittler (aus Vault-Erfahrung) | -2 |
| Unklare Vergütung oder "nach Vereinbarung" ohne Eckdaten | -1 |
| Sehr kurze Reaktionsfrist (<24h) | -1 |
| Branche mit hohem Konfliktpotenzial | -1 |

### 6. Strategischer Wert (10%)

| Score | Kriterien |
|-------|-----------|
| 5 | Türöffner für neue Branche/Kunde + Referenzwert + Lernpotenzial |
| 4 | Starker Referenzwert ODER hohes Lernpotenzial |
| 3 | Solider Auftrag, normaler Referenzwert |
| 2 | Wenig Neues, reine Wiederholung bekannter Aufgaben |
| 1 | Kein strategischer Mehrwert, reines "Geld verdienen" |

**Bonus-Faktoren:**
- Neue Branche erschließen: +1
- KI + Scrum Kombination (USP stärken): +1
- Folgeauftrag-Potenzial: +0.5
- Schiene-B-Synergie (KMU-Produkt-Ideen): +0.5

---

## Tracker-Format

Datei: `pipeline/tracker.md`

```markdown
| Datum | Kunde | Rolle | Vermittler | Score | Status | Stundensatz | Pfad |
```

### Status-Werte

| Status | Bedeutung |
|--------|-----------|
| `Neu` | Gerade entdeckt, noch nicht bewertet |
| `Bewertet` | Scoring durchgeführt, Entscheidung offen |
| `In Bearbeitung` | /anfrage läuft (Profil wird erstellt) |
| `Eingereicht` | Profil/Angebot beim Kunden/Vermittler |
| `Erstgespräch` | Gespräch vereinbart oder stattgefunden |
| `Verhandlung` | Konditionen werden verhandelt |
| `Gewonnen` | Auftrag erhalten |
| `Verloren` | Absage vom Kunden/Vermittler |
| `Abgelehnt` | Roland hat abgelehnt |
| `Geparkt` | Interessant, aber aktuell keine Kapazität |

---

## Positionierung

Roland ist der pragmatische Macher, der KI und Agilität zusammenbringt:
- **Alleinstellungsmerkmal:** Scrum Master MIT KI-Expertise — selten am Markt
- **Erfahrung:** 25+ Jahre agile Softwareentwicklung, 17+ Jahre Scrum Master
- **Branchen:** Finanzsektor (KfW, Commerzbank), Automotive (CARIAD/VW), Handel (REWE)
- **Zwei Standbeine:** Freelancer-Aufträge (Schiene A) + eigene Produkte (Schiene B)
