---
name: workspace-check
description: Prüft den Claude Code Workspace auf Vollständigkeit — gleicht Skills, Agents, MCP-Server und Memory mit der Obsidian-Doku und dem GitHub-Repo ab, meldet Lücken und schließt sie.
user_invocable: true
---

# Workspace-Check

Du prüfst den gesamten Claude Code Workspace auf Konsistenz und Vollständigkeit. Ziel: Alles ist dokumentiert, synchronisiert und der zentrale Index ist aktuell.

## Schritt 1: Workspace scannen

Ermittle den aktuellen Stand aus `~/.claude/`:

- **Skills:** Alle Ordner unter `~/.claude/skills/` mit SKILL.md
- **Agents:** Alle `.md`-Dateien unter `~/.claude/agents/`
- **MCP-Server:** Konfigurierte Server in `~/.claude.json` (Abschnitt `mcpServers`)
- **Memory:** Alle Projekte unter `~/.claude/projects/*/memory/` die `.md`-Dateien enthalten
- **Settings:** `~/.claude/settings.json` und `~/.claude/CLAUDE.md`
- **Projekte:** Alle Projekt-CLAUDE.md-Dateien unter `C:/opt/Projects/clause-code-ws/`

Erstelle eine Bestandsliste mit allen gefundenen Elementen.

## Schritt 2: Obsidian-Doku abgleichen

Vergleiche die Bestandsliste mit der Dokumentation im Mentronig Vault:

- **Index:** `C:\opt\Projects\obsidian-ws\Mentronig\04 Ressourcen\KI\Claude Code.md` — sind alle Skills, Agents, MCP-Server und Projekte aufgeführt?
- **Skill-Dokus:** Für jeden Skill unter `~/.claude/skills/` muss eine `.md`-Datei unter `04 Ressourcen/KI/Claude Code/Skills/` existieren
- **Agents-Doku:** `04 Ressourcen/KI/Claude Code/Agents.md` — sind alle Agents aufgeführt?
- **MCP-Server-Doku:** Sind alle konfigurierten MCP-Server dokumentiert?

Erstelle eine Liste mit Abweichungen:
- Neue, undokumentierte Elemente
- Dokumentierte Elemente, die nicht mehr existieren
- Veraltete Informationen (z.B. geänderter Agent-Name oder Modell)

## Schritt 3: GitHub-Repo abgleichen

Vergleiche die Bestandsliste mit dem lokalen Repo `C:/opt/Projects/clausde-resources/`:

- Fehlen neue Skills oder Agents im Repo?
- Sind bestehende Dateien im Repo veraltet (älter als die Quelle in `~/.claude/`)?

Falls das Repo lokal nicht existiert: Hinweis geben, dass `/sync-config` ausgeführt werden sollte.

## Schritt 4: Report ausgeben

Zeige dem User einen strukturierten Report:

```
## Workspace-Check Report

### Bestandsaufnahme
- Skills: X gefunden
- Agents: X gefunden
- MCP-Server: X gefunden
- Projekte mit Memory: X gefunden

### Obsidian-Doku
- [OK] Skill xyz dokumentiert
- [FEHLT] Skill abc — keine Doku unter Skills/
- [VERALTET] Agent xyz — Modell hat sich geändert

### GitHub-Repo
- [OK] Alles synchron
- [VERALTET] Agent xyz — lokale Version neuer als Repo
- [FEHLT] Skill abc — nicht im Repo

### Empfohlene Aktionen
1. Doku erstellen für Skill abc
2. /sync-config ausführen
3. Index aktualisieren
```

## Schritt 5: Lücken schließen

Frage den User: **"Soll ich die Lücken jetzt schließen?"**

Falls ja, führe die nötigen Aktionen aus:

- **Fehlende Skill-Doku:** Neue `.md`-Datei unter `04 Ressourcen/KI/Claude Code/Skills/` erstellen (Format wie bestehende Skill-Dokus: Frontmatter, Aufruf, Ablauf-Tabelle, Speicherort)
- **Fehlende Agent-Doku:** `Agents.md` um den neuen Agent ergänzen
- **Veralteter Index:** `Claude Code.md` aktualisieren
- **Entfernte Elemente:** Aus der Doku entfernen
- **GitHub nicht synchron:** `/sync-config` Skill aufrufen bzw. den User darauf hinweisen

## Schritt 6: Änderungen dokumentieren

Aktualisiere die betroffenen Dateien im Mentronig Vault:

- **Index:** `04 Ressourcen/KI/Claude Code.md` — neue Skills, Agents, MCP-Server oder Projekte eintragen, entfernte streichen
- **Agents.md:** Neue Agents hinzufügen oder entfernte streichen
- **Skill-Dokus:** Neue Skill-Dokumentationen unter `04 Ressourcen/KI/Claude Code/Skills/` erstellen
- **Backup & Sync.md:** Falls sich die Backup-Struktur geändert hat, aktualisieren

Am Ende eine kurze Zusammenfassung ausgeben, welche Dateien im Vault geändert oder erstellt wurden.

## Wichtige Regeln

- **Nur lesen und vergleichen** in den Schritten 1–4 — nichts ändern ohne User-Bestätigung
- **Keine Credentials** prüfen oder ausgeben
- **Obsidian-Links** im Wikilink-Format verwenden (`[[Seitenname]]`)
- **Frontmatter** bei neuen Dateien immer mit tags und Erstelldatum
