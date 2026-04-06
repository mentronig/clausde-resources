---
name: youtube-transkript
description: Transkribiert ein YouTube-Video (Audio herunterladen via yt-dlp, transkribieren via Whisper, als Markdown speichern)
user_invocable: true
args: <youtube-url-oder-video-id>
---

# YouTube-Video transkribieren

Du transkribierst ein YouTube-Video und speicherst das Ergebnis als Markdown-Datei.

## Eingabe

Der User übergibt eine YouTube-URL oder Video-ID als Argument. Extrahiere daraus die 11-stellige Video-ID.

Beispiele:
- `https://www.youtube.com/watch?v=dQw4w9WgXcQ` → `dQw4w9WgXcQ`
- `https://youtu.be/dQw4w9WgXcQ` → `dQw4w9WgXcQ`
- `dQw4w9WgXcQ` → `dQw4w9WgXcQ`
- URL mit Zeitstempel `&t=811s` → ignorieren, nur die ID extrahieren

## Schritt 1: Video-Metadaten abrufen

Rufe mit dem YouTube-MCP-Tool `mcp__youtube__videos_getVideo` die Metadaten des Videos ab (Titel, Kanal, Veröffentlichungsdatum). Diese werden für die Markdown-Datei benötigt.

## Schritt 2: Audio herunterladen

Lade die Audio-Spur mit `yt-dlp` herunter. Auf Windows `python -m yt_dlp` verwenden, falls `yt-dlp` nicht direkt im PATH ist.

```bash
# Variante 1: direkt
yt-dlp -x --audio-format mp3 -o "/tmp/yt-transkript-%(id)s.%(ext)s" "https://www.youtube.com/watch?v=<VIDEO_ID>"

# Variante 2: über Python-Modul (Windows-Fallback)
python -m yt_dlp -x --audio-format mp3 -o "/tmp/yt-transkript-%(id)s.%(ext)s" "https://www.youtube.com/watch?v=<VIDEO_ID>"
```

Falls `ffmpeg` nicht installiert ist, wird die Datei als `.webm` statt `.mp3` gespeichert — das funktioniert trotzdem mit Whisper.

Prüfe, ob die Datei erfolgreich erstellt wurde (ggf. mit `.webm`-Endung).

## Schritt 3: Audio transkribieren

Transkribiere die Audio-Datei mit Whisper. Je nach Plattform die passende Variante verwenden:

### macOS (Apple Silicon) — mlx-whisper

```bash
python3 -c "
import mlx_whisper

result = mlx_whisper.transcribe(
    '/tmp/yt-transkript-<VIDEO_ID>.mp3',
    path_or_hf_repo='mlx-community/whisper-large-v3-turbo',
    language='de'
)
print(result['text'])
"
```

### Windows / Linux — faster-whisper

```bash
python -c "
import os
from faster_whisper import WhisperModel

audio_path = os.path.join(os.environ.get('TEMP', '/tmp'), 'yt-transkript-<VIDEO_ID>.webm')
# Falls .mp3 existiert, diese verwenden
if not os.path.exists(audio_path):
    audio_path = audio_path.replace('.webm', '.mp3')

model = WhisperModel('large-v3-turbo', device='cpu', compute_type='int8')
segments, info = model.transcribe(audio_path, language='de')

for segment in segments:
    print(segment.text, end='')
"
```

**Hinweise:**
- Falls das Video nicht deutschsprachig ist, passe `language` entsprechend an oder frage den User.
- Auf Windows den nativen Temp-Pfad über `os.environ['TEMP']` verwenden, da `/tmp/` anders aufgelöst wird.
- Installation falls nötig: `pip install faster-whisper` (Windows/Linux) oder `pip install mlx-whisper` (macOS).

## Schritt 4: Temporäre Datei aufräumen

Lösche die heruntergeladene Audio-Datei:

```bash
rm /tmp/yt-transkript-<VIDEO_ID>.*
```

## Schritt 5: Markdown-Datei speichern

Speichere das Transkript als Markdown-Datei unter:

```
C:\opt\Projects\obsidian-ws\Mentronig\08 Speicher\yt-transkript_<VIDEO_ID>.md
```

**Format der Markdown-Datei:**

```markdown
---
title: <Video-Titel>
channel: <Kanal-Name>
date: <Veröffentlichungsdatum>
video_id: <VIDEO_ID>
url: https://www.youtube.com/watch?v=<VIDEO_ID>
transcribed: <heutiges Datum>
---

# <Video-Titel>

<Transkript-Text>
```

## Wichtige Regeln

- **Umlaute:** ö, ä, ü, ß korrekt ausschreiben
- **Sprache:** Standard ist Deutsch (`language='de'`). Bei anderssprachigen Videos den User fragen.
- **Fehlerbehandlung:** Falls `yt-dlp` oder Whisper nicht installiert ist, automatisch per `pip install` nachinstallieren. Bei Fehler den User darauf hinweisen.
- **Ausgabe:** Am Ende den Pfad zur gespeicherten Datei und eine kurze Zusammenfassung (Titel, Dauer, Sprache) ausgeben.
