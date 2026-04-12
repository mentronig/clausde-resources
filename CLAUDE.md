# Globale Regeln

## Plan-Modus

- Wenn du im Plan-Modus bist, muss der Plan IMMER erst vom User abgesegnet werden (via ExitPlanMode), bevor mit der Implementierung begonnen wird.
- Setze den Plan NIEMALS eigenstaendig um, ohne dass der User ihn explizit bestaetigt hat.

## Texte und Sprache

- Umlaute (ö, ä, ü, Ö, Ä, Ü) und ß IMMER korrekt ausschreiben – niemals als oe, ae, ue, ss ersetzen.

## Modell-Wahl

- Nutze **Sonnet** (haiku/sonnet subagents) für schnelle, einfache Aufgaben.
- Nutze **Opus 4.6** für komplexe Architektur-Entscheidungen und anspruchsvolle Tasks.
- <!-- TEMP bis 2026-04-22: danach diesen Block entfernen -->
  **Modell-Verifizierung (vor jeder Aufgabe):** Kurz prüfen ob Sonnet oder Opus passt — einen Satz Begründung ausgeben, bevor mit der Aufgabe begonnen wird.
  - Opus: komplexe Architektur, schwieriges Debugging mit unklarer Ursache, mehrstufige Refactorings, tiefes Reasoning
  - Sonnet: Dokumentation, einfache Installationen, Config, Memory, Vault-Organisation, Erklärungen, geradlinige Code-Änderungen
  <!-- /TEMP -->

## Skills

- Alle Skills werden global unter `~/.claude/skills/` gespeichert (Ordner mit SKILL.md + optionalem references/-Ordner).
- Backup/Versionierung aller Skills im GitHub-Repo: https://github.com/mentronig/clausde-resources/skills/

## Memory

- **Memory ist IMMER aktiv** – in jedem Projekt, in jeder Session.
- Lese zu Beginn jeder Session **zuerst** `~/.claude/memory/MEMORY.md` (globales Memory), dann das Projekt-Memory-Verzeichnis. <!-- TEMP bis 2026-04-22: danach diese Zeile entfernen -->
- Lese danach immer `C:\opt\Projects\obsidian-ws\Mentronig\00 Kontext\hot.md` — das ist der Vault-Hot-Cache mit dem projektübergreifenden Zustand (offene Fäden, nächste Schritte).
- Prüfe zu Beginn jeder Session, ob `MEMORY.md` im Projekt-Memory-Verzeichnis existiert.
- Falls nicht: Erstelle das Verzeichnis und eine leere `MEMORY.md` automatisch, ohne dass der User danach fragen muss.
- Speichere relevante Erkenntnisse proaktiv in Memory – warte nicht darauf, dass der User "speichere das" sagt.
- Am Ende einer Session, wenn substanzielle Arbeit geleistet wurde, aktualisiere `MEMORY.md` mit dem aktuellen Stand.
- **Aufräumen**: Wenn du Memory-Einträge liest, prüfe ob sie noch aktuell sind. Veraltete oder falsche Einträge sofort korrigieren oder entfernen.

## Agenten

- Der **HR-Agent** (`~/.claude/agents/hr-agent.md`) stellt für neue Projekte Agenten-Teams zusammen.
- Er führt ein Interview, erstellt einen Plan (muss bestätigt werden), und richtet alle Dateien ein.
- Seine Erfahrungen speichert er in `~/.claude/agents/hr-erfahrungen.md`.

## Transkription

- Standard-Modell für Audio-Transkription: **mlx-whisper** mit `mlx-community/whisper-large-v3-turbo`.
- Immer `language="de"` setzen bei deutschsprachigen Inhalten.

## Web-Scraping Fallback

Beim Abrufen von Webseiten gilt diese Reihenfolge:

1. **WebFetch** — immer zuerst versuchen (kostenlos, kein Limit)
2. **Firecrawl MCP** (`mcp__firecrawl__firecrawl_scrape`) — Fallback wenn:
   - WebFetch liefert leere, unvollständige oder unbrauchbare Ergebnisse
   - Seite erfordert JavaScript-Rendering (SPA, dynamische Inhalte)
   - WebFetch landet auf einer Login-Seite statt dem Zielinhalt
   - Bessere Markdown-Qualität explizit benötigt wird
   - **Beim Wechsel immer kurz hinweisen:** *„WebFetch hat kein brauchbares Ergebnis geliefert — wechsle zu Firecrawl."*

**Wenn Firecrawl nicht verfügbar ist** (Tool nicht in der Session geladen):
- Weise den User darauf hin: *„Firecrawl MCP ist in dieser Session nicht verfügbar."*
- Lies den API-Key aus der Windows-Systemvariablen und registriere Firecrawl:
  ```bash
  claude mcp add firecrawl -e FIRECRAWL_API_KEY=$env:FIRECRAWL_API_KEY -- npx -y firecrawl-mcp
  ```
- Der API-Key ist in der Windows-Systemvariablen `FIRECRAWL_API_KEY` hinterlegt — nicht manuell eingeben.
- Falls die Variable nicht gesetzt ist: User darauf hinweisen, den Key unter https://firecrawl.dev zu holen und als Windows-Systemvariable `FIRECRAWL_API_KEY` zu setzen.
- Fahre mit WebFetch fort, solange Firecrawl nicht verfügbar ist.

