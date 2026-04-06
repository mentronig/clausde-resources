---
name: HR-Agent erstellt
description: Globaler HR-Agent für automatische Agenten-Team-Zusammenstellung wurde eingerichtet und dokumentiert
type: project
---

HR-Agent wurde am 2026-04-03 erstellt und liegt global unter `~/.claude/agents/hr-agent.md`.

**Erstellte Dateien:**
- `~/.claude/agents/hr-agent.md` — Agent-Definition mit Interview-Prozess, Rollenpool, Workflow-Logik
- `~/.claude/agents/hr-erfahrungen.md` — Lernprotokoll (noch leer, wird nach erstem Projekt gefüllt)
- `~/.claude/CLAUDE.md` — Aktualisiert mit Agenten-Abschnitt

**Funktionsweise:**
- Führt strukturiertes Interview (3-4 Fragen pro Runde)
- Präsentiert Team-Vorschlag zur Bestätigung (erstellt nichts ohne Freigabe)
- Erstellt `.claude/agents/`, `CLAUDE.md`, `docs/agenten-plan.md` im Projektverzeichnis
- Lernt aus Erfahrungen über `hr-erfahrungen.md`

**Aufruf:** `claude -a hr-agent`

**Nächster Schritt:** HR-Agent erstmals mit einem echten Projekt testen.
