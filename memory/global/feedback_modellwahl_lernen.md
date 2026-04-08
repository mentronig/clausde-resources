---
name: Modellwahl vor jeder Aufgabe prüfen
description: Temporär (bis 2026-04-22) vor jeder Aufgabe prüfen ob Opus oder Sonnet passend ist, mit kurzer Begründung
type: feedback
---

Vor jeder Aufgabe kurz prüfen, ob das aktuell geladene Modell zur Aufgabe passt. Falls nicht, einen Wechsel vorschlagen mit einer einzeiligen Begründung.

**Why:** Roland lernt gerade, welche Aufgaben welche Modellstärke brauchen. Die Empfehlung soll ihm ein Gefühl dafür vermitteln, wann Opus nötig ist und wann Sonnet reicht.

**How to apply:**
- Vor jeder neuen Aufgabe einen kurzen Hinweis geben (ein Satz, keine Diskussion)
- Opus: komplexe Architektur, schwieriges Debugging mit unklarer Ursache, mehrstufige Refactorings, tiefes Reasoning
- Sonnet: Dokumentation, einfache Installationen, Config, Memory, Vault-Organisation, geradlinige Code-Änderungen, Umsetzung nach klarer Diagnose
- Bei Debugging: Opus für die Diagnose, dann Wechsel zu Sonnet sobald Ursache klar ist
- Globales Memory zu Sessionbeginn lesen — nicht nur Projekt-Memory
- **Ablaufdatum: 2026-04-22** — danach:
  1. Diese Datei löschen
  2. In `~/.claude/CLAUDE.md` die Zeile mit `TEMP bis 2026-04-22` entfernen
  3. Roland fragen, ob er die Modellwahl-Regel weiter braucht
