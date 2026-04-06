---
name: Session 2026-04-03 – Projektstatus
description: Gesamtübersicht aller Ergebnisse der Einrichtungssession am 2026-04-03
type: project
---

## Was wurde aufgebaut

### Agenten-Team (global unter ~/.claude/agents/)
- **Akquise-Agent** (Opus 4.6) – Kundenrecherche, Pitches für KI + Scrum Master
- **Kommunikations-Agent** (Sonnet 4.6) – Profile, Profilanpassungen, E-Mails, Newsletter
- **Content-Agent** (Sonnet 4.6) – Blog, Social Media, Case Studies
- **Website-Agent** (Sonnet 4.6) – mentronig.com Pflege, HTML/CSS/JS

### Skill
- **`/anfrage`** (global unter ~/.claude/skills/anfrage/) – Startet den 4-Schritte-Anfragen-Workflow

### Verzeichnisstruktur
```
mentronig/
├── inbox/          – Eingang für neue Aufgaben
├── outbox/         – Versandfertige Dateien
├── auftraege/      – Kundenanfragen (YYYY-MM_kunde-projekt/)
│   └── 2026-04_kfw-disco-scrum-master/ (erster Auftrag, komplett bearbeitet)
├── profile/        – Stammprofile + Vorlagen
│   ├── Lebenslauf.docx + .md
│   ├── Kurzprofil.md (optimiert mit ETL/Informatica, DevSecOps)
│   ├── profil-ki.md (NEU – gesamtes KI-Portfolio)
│   └── vorlagen/meta-system/
├── docs/           – Workflows, Pläne
└── CLAUDE.md       – Projektregeln
```

### Profile
- **Kurzprofil.md** – Optimiert: ETL/Informatica, DevSecOps-Pilotprojekt, Datenarchitektur-Keywords, Riskreporting-Projekt ergänzt
- **Lebenslauf.md** – Text-Spiegel der vollständigen Projekthistorie seit 1991
- **profil-ki.md** – KI-Expertise-Profil mit 5 Projekten (KfW PoC, Website Rebuild, MVP Projektassistent, Wissensagent, Agenten-System mentronig)

### KfW DISCO Auftrag (komplett bearbeitet)
- Anfrage-Analyse erstellt
- Profil-Abgleich erstellt (alle 12 Muss + 2 Kann erfüllt)
- PROFIL-ROLAND pptx optimiert (ETL/Informatica, DevSecOps, Datenarchitektur)
- Angebotsfrist: 10.04.2026
- Möglicher Tagessatz: 680 € (85 €/h)

### Regeln etabliert
- Text-Spiegel für alle Binärdokumente (CLAUDE.md Regel 6)
- Anfragen-Workflow dokumentiert (docs/workflow-anfrage.md)
- Memory aktiv in jeder Session
