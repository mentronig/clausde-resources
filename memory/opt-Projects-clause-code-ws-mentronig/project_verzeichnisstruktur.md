---
name: Verzeichnisstruktur mentronig
description: Projektstruktur mit inbox/outbox-Workflow und Auftragsablage nach YYYY-MM_kunde-projekt
type: project
---

Festgelegte Verzeichnisstruktur für mentronig:

- **inbox/** – Eingang für alle Dateien, die für eine Aufgabe benötigt werden
- **outbox/** – Fertige Dateien, die zum Versenden geeignet sind
- **auftraege/** – Kundenanfragen/Aufträge, benannt nach `YYYY-MM_kunde-projekt`
  - **anfrage/** – Eingehende Dokumente (Vergabeunterlagen, Ausschreibungen)
  - **angebot/** – Eigene Antwort (angepasstes Profil, Angebotsdokumente)
- **profile/** – Stammprofile (Lebenslauf.docx, Kurzprofil.md)
  - **vorlagen/meta-system/** – Profilformat für Vermittler meta-system (pptx)
- **docs/** – Projektdokumentation (u.a. agenten-plan.md)

**Why:** Roland arbeitet als Ein-Mann-Unternehmen und braucht eine einfache, konsistente Ablage.

**How to apply:** Neue Aufträge immer unter `auftraege/YYYY-MM_kunde-projekt/` anlegen. Dateien von meta-system verwenden das pptx-Format aus `profile/vorlagen/meta-system/`.
