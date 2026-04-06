# Globale Regeln

## Plan-Modus

- Wenn du im Plan-Modus bist, muss der Plan IMMER erst vom User abgesegnet werden (via ExitPlanMode), bevor mit der Implementierung begonnen wird.
- Setze den Plan NIEMALS eigenstaendig um, ohne dass der User ihn explizit bestaetigt hat.

## Texte und Sprache

- Umlaute (ö, ä, ü, Ö, Ä, Ü) und ß IMMER korrekt ausschreiben – niemals als oe, ae, ue, ss ersetzen.

## Modell-Wahl

- Nutze **Sonnet** (haiku/sonnet subagents) für schnelle, einfache Aufgaben.
- Nutze **Opus 4.6** für komplexe Architektur-Entscheidungen und anspruchsvolle Tasks.

## Skills

- Alle Skills werden global unter `~/.claude/skills/` gespeichert (Ordner mit SKILL.md + optionalem references/-Ordner).
- Backup/Versionierung aller Skills im GitHub-Repo: https://github.com/mentronig/clausde-resources/skills/

## Memory

- **Memory ist IMMER aktiv** – in jedem Projekt, in jeder Session.
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

