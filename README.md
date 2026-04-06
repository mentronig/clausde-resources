# Claude Code Konfiguration

Backup der globalen Claude Code Konfiguration aus `~/.claude/`.

## Inhalt

```
├── CLAUDE.md                        # Globale Regeln & Standards
├── settings.json                    # Modell, Sprache, Einstellungen
├── agents/                          # Agent-Definitionen
│   ├── hr-agent.md                  # Team-Builder für neue Projekte
│   ├── akquise-agent.md             # Kundengewinnung
│   ├── content-agent.md             # Blog & Social Media
│   ├── kommunikations-agent.md      # Profile & E-Mails
│   ├── website-agent.md             # mentronig.com
│   └── hr-erfahrungen.md            # Lernprotokoll des HR-Agenten
├── skills/                          # Custom Skills
│   ├── anfrage/SKILL.md             # Kundenanfragen-Workflow
│   └── youtube-transkript/SKILL.md  # YouTube-Video-Transkription
└── memory/                          # Projekt-Memory
    ├── mentronig/                   # Business-Projekt
    └── tools-claude-code/           # Claude Code Tooling
```

## Wiederherstellung auf neuem Rechner

### 1. Repo klonen

```bash
cd ~/
gh repo clone mentronig/clausde-resources
```

### 2. Dateien an die richtige Stelle kopieren

```bash
# Globale Konfiguration
cp clausde-resources/CLAUDE.md ~/.claude/CLAUDE.md
cp clausde-resources/settings.json ~/.claude/settings.json

# Agents
cp clausde-resources/agents/*.md ~/.claude/agents/

# Skills
mkdir -p ~/.claude/skills/anfrage ~/.claude/skills/youtube-transkript
cp clausde-resources/skills/anfrage/SKILL.md ~/.claude/skills/anfrage/
cp clausde-resources/skills/youtube-transkript/SKILL.md ~/.claude/skills/youtube-transkript/

# Memory (optional, projektspezifisch)
# Pfade an das neue System anpassen!
```

### 3. Neu authentifizieren

Credentials werden nicht gesichert. Nach der Wiederherstellung:
- `claude` starten und einloggen
- MCP-Server (Gmail, Calendar) neu authentifizieren

## Aktualisierung

Nach Änderungen an Agents, Skills oder CLAUDE.md:

```bash
cd C:/opt/Projects/clausde-resources

# Geänderte Dateien kopieren (Beispiel: Agent aktualisiert)
cp ~/.claude/agents/akquise-agent.md agents/

# Committen & pushen
git add -A && git commit -m "Agent aktualisiert: akquise-agent"
git push
```
