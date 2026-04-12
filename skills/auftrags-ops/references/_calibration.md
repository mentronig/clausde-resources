# Auftrags-Ops — Kalibrierungsprotokoll

Gesammelte Erkenntnisse aus Scoring-Vergleichen (Vorhersage vs. tatsächlicher Outcome).
Wird beim Scoring **implizit** berücksichtigt — nicht als feste Regeländerung, sondern als Reasoning-Kontext.

## Wie dieses Protokoll verwendet wird

Beim Scoring (bewerten / scan) liest das System dieses Protokoll und fragt sich:
- Gibt es ähnliche frühere Fälle (Vermittler, Rollentyp, Branche)?
- Was wurde damals falsch eingeschätzt?
- Welche Dimension braucht für diesen Fall eine Anpassung?

## Format für neue Einträge

```
### [DATUM] — [Kunde / Rolle] → [Note vorher] → [Outcome]
**Scoring-Vorhersage:** [Note] ([Score]/5)
**Tatsächlicher Outcome:** [Gewonnen / Verloren / Abgelehnt / Kein Feedback]
**Abweichung:** [Was hat die Vorhersage verfehlt?]
**Ursache:** [Stundensatz zu hoch / Profil-Lücke / Timing / Konkurrenz / Marktbedingung / ...]
**Lektion:** [Konkrete Faustregel für ähnliche zukünftige Fälle]
**Betroffene Dimension:** [Rollenpassung / Kommerz / Kundenfit / Logistik / Red Flags / Strategischer Wert]
```

---

## Einträge

_Noch keine Einträge. Erster Eintrag entsteht nach dem KfW DISCO Outcome._

---

## Auto-Reject-Schwellenwert

**Aktueller Schwellenwert: 3,8**
Projekte mit Global Score < 3,8 werden automatisch als `Abgelehnt` in den Tracker eingetragen — keine manuelle Entscheidung nötig.

Schwellenwert kann im Weekly Review angepasst werden (nach oben bei voller Pipeline, nach unten bei Auslastungslücke).

---

## Weekly Review — Vorschläge für _shared.md

Wird wöchentlich ausgeführt wenn neue Einträge vorhanden sind. Destilliere die Lektionen und schlage Roland konkrete Gewichtsänderungen oder Benchmark-Anpassungen in `_shared.md` vor.

Vorlage für den Review-Output:
```
Kalibrierungs-Review — KW [XX] / [Datum]

Basis: [N] neue Einträge seit letztem Review ([Datum])
Auto-Reject-Schwellenwert: [aktuell X,X]

Muster dieser Woche:
- [Beobachtung 1]
- [Beobachtung 2]

Vorgeschlagene Anpassungen:
1. [Dimension X] — Gewicht von Y% auf Z% erhöhen
   Begründung: [Muster aus Einträgen]

2. [Benchmark Stundensatz] — Anpassen von X€ auf Y€
   Begründung: [Muster aus Einträgen]

3. [Auto-Reject-Schwellenwert] — Anpassen von X,X auf Y,Y?
   Begründung: [Pipeline-Auslastung / Marktlage]

Bitte bestätigen (ja/nein/anpassen) — erst dann wird _shared.md geändert.
```

---

## Prozess-Reflexion (Ebene 2)

Ergänzung zum Scoring-Kalibrierungsloop. Erfasst Erkenntnisse über den **Workflow selbst** — nicht über Scoring-Outcomes, sondern über Prozessschritte die falsch, fehlend oder umständlich waren.

### Wie dieses Protokoll verwendet wird

Beim Weekly Review werden beide Ebenen gemeinsam betrachtet:
- **Ebene 1:** Scoring-Outcomes (war die Vorhersage richtig?)
- **Ebene 2:** Prozess-Reflexion (hat der Workflow gut funktioniert?)

Jeder Eintrag endet mit einer **konkreten Dateiänderung** — erst dann ist der Loop geschlossen.

### Format für neue Einträge

```
### [DATUM] — [Kurzbeschreibung des Problems]
**Problem:** Was war umständlich / falsch / fehlend?
**Ursache:** Warum war das so? (Annahme im Skill, fehlendes Feature, fehlende Info)
**Änderung:** Konkrete Anpassung in [Dateiname, Abschnitt]
**Status:** ✅ Umgesetzt / ⏳ Offen / ❌ Bewusst nicht umgesetzt (Begründung)
```

### Trigger

Ein Prozess-Reflexions-Eintrag wird angelegt wenn:
- Ein Workflow-Schritt manuell korrigiert werden musste
- Roland eine Änderung am Prozess vorschlägt oder bestätigt
- Ein Schritt wiederholt Aufwand verursacht, der automatisierbar wäre
- Ein neues Muster über mehrere Anfragen erkennbar wird

---

## Prozess-Reflexions-Einträge

### 2026-04-12 — freelancermap Projektagent liefert zu viele Nicht-SM-Rollen
**Problem:** Alert "Scrum Master – 7 neue Projekte" enthielt 0 SM-Rollen: Risk Manager (3×), Developer (2×), PL (1×), SAP QA (1×). 100% Auto-Reject, Scan-Wert = null.
**Ursache:** Projektagent-Suchbegriff vermutlich zu weit gefasst (Keyword-Matching statt exakter Rollenfilter) — freelancermap matcht auch auf Teilwörter oder verwandte Begriffe.
**Änderung:** Projektagent auf freelancermap enger konfigurieren — Anleitung im Kalibrierungsprotokoll. Ausschluss-Keywords hinzufügen.
**Status:** ⏳ Offen — Roland setzt Änderung direkt auf freelancermap.de um

### 2026-04-10 — Stundensatz-Scoring bei Erstanfragen strukturell fehlerhaft
**Problem:** Bei Erstanfragen über Plattformen ist der Stundensatz strukturell unbekannt — das alte Schema bestrafte das mit Score 2 auf 20% Gewicht. Führte zu systematisch zu niedrigen Scores.
**Ursache:** Kommerz-Dimension war ursprünglich auf Stundensatz als primäres Signal ausgelegt — passt nicht zum Plattform-Workflow.
**Änderung:** `_shared.md` — Kommerz zweistufig: Stufe 1 bewertet nur Laufzeit/Volumen, Stundensatz als Korrekturfaktor nach Vermittler-Kontakt.
**Status:** ✅ Umgesetzt 2026-04-10

### 2026-04-10 — Logistik-Scoring zu starr (Reisen pauschal bestraft)
**Problem:** Logistik-Score 2 für alles außerhalb Rhein-Main — obwohl Reisen für Roland OK ist wenn Stundensatz stimmt.
**Ursache:** Schema war auf tägliches Pendeln ausgelegt, nicht auf Hotel/Projektreisen.
**Änderung:** `_shared.md` — Logistik-Schema überarbeitet: Reisen bundesweit bei ≥100 €/h = Score 3, Ausland non-DACH = Hard-KO.
**Status:** ✅ Umgesetzt 2026-04-10

### 2026-04-10 — Mail-Scan bewertet nur Kurzinfos, nicht Detailseiten
**Problem:** Freelance-Plattform-Mails enthalten nur Titel + Ort — vollständiges Scoring auf dieser Basis ist unzuverlässig (z.B. SM BI Hannover: erst nach WebFetch zeigte sich ~100% Remote).
**Ursache:** Scan-Mode war auf Mail-Content ausgelegt, kein zweistufiger Prozess vorgesehen.
**Änderung:** `SKILL.md` (auftrags-ops, Scan-Mode) — zweistufige Architektur einführen: Stufe 1 Mail-Vorfilter (Rollenpassung + harte KOs), Stufe 2 WebFetch Detail-Scan für Kandidaten.
**Status:** ⏳ Offen — Skill-Update nach erstem Test-Durchlauf geplant

### 2026-04-10 — Vault-Inbox-Datei blieb nach Anfrage-Workflow liegen
**Problem:** Nach Bearbeitung einer Anfrage verblieb die Originaldatei in `01 Inbox/Anfragen/` — kein automatischer Aufräumschritt im Workflow.
**Ursache:** Schritt fehlte in `anfrage/SKILL.md`.
**Änderung:** `anfrage/SKILL.md` — Schritt 1 ergänzt: Originaldatei nach `08 Speicher/Anfragen/` verschieben.
**Status:** ✅ Umgesetzt 2026-04-10

### 2026-04-10 — Kontakt-E-Mail in Bewerbungstexten falsch
**Problem:** Bewerbungstexte enthielten `info@mentronig.com` statt `roland.glienke@t-online.de`.
**Ursache:** Kurzprofil-Vorlage verwendet mentronig-E-Mail — für Vermittler-Kontakt ist aber die private Adresse korrekt.
**Änderung:** Memory `feedback_bewerbung_kontakt.md` — Regel gespeichert. Kurzprofil-Scripts und Vorlagen künftig mit `roland.glienke@t-online.de`.
**Status:** ✅ Umgesetzt 2026-04-10
