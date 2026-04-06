---
name: sync-config
description: Synchronisiert die Claude Code Konfiguration (CLAUDE.md, Agents, Skills, Memory, Settings) ins GitHub-Repo clausde-resources
user_invocable: true
---

# Claude Code Konfiguration synchronisieren

Synchronisiere alle portablen Claude Code Konfigurationsdateien ins GitHub-Repo `mentronig/clausde-resources`.

## Schritt 1: Dateien kopieren

Kopiere alle relevanten Dateien aus `~/.claude/` ins lokale Repo `C:/opt/Projects/clausde-resources/`:

```bash
# Globale Konfiguration
cp ~/.claude/CLAUDE.md C:/opt/Projects/clausde-resources/CLAUDE.md
cp ~/.claude/settings.json C:/opt/Projects/clausde-resources/settings.json

# Agents
cp ~/.claude/agents/*.md C:/opt/Projects/clausde-resources/agents/

# Skills — für jeden Skill-Ordner den Inhalt kopieren
```

Für Skills: Prüfe welche Skill-Ordner unter `~/.claude/skills/` existieren. Für jeden Ordner:
- Erstelle den Zielordner unter `clausde-resources/skills/<name>/` falls nötig
- Kopiere alle Dateien (SKILL.md und ggf. references/)

Für Memory: Prüfe welche Projekte unter `~/.claude/projects/*/memory/` Memory-Dateien haben. Für jedes Projekt:
- Leite den Kurznamen aus dem Projekt-Pfad ab (z.B. `C--opt-Projects-clause-code-ws-mentronig` → `mentronig`)
- Erstelle den Zielordner unter `clausde-resources/memory/<kurzname>/` falls nötig
- Kopiere alle `.md`-Dateien

## Schritt 2: Änderungen prüfen

Führe `git status` im Repo aus. Falls keine Änderungen vorhanden sind: Meldung ausgeben und fertig.

## Schritt 3: Commit & Push

Falls Änderungen vorhanden:
1. Zeige dem User eine kurze Zusammenfassung der Änderungen (neue/geänderte/gelöschte Dateien)
2. Erstelle einen Commit mit einer aussagekräftigen Message, die die Änderungen beschreibt
3. Pushe zu GitHub

## Schritt 4: Bestätigung

Gib eine kurze Zusammenfassung aus:
- Anzahl geänderter Dateien
- Link zum Repo

## Wichtige Regeln

- **Niemals** `.credentials.json`, `history.jsonl`, `sessions/` oder andere sensitive Dateien kopieren
- **Niemals** `git add -A` verwenden — nur die bekannten Dateien/Ordner hinzufügen
- Das Repo liegt unter `C:/opt/Projects/clausde-resources/`
- Falls das Repo lokal nicht existiert: `gh repo clone mentronig/clausde-resources` unter `C:/opt/Projects/` ausführen
