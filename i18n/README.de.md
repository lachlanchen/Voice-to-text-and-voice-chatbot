[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Voice-to-Voice-Chatbot mit Whisper, LLaMA und Groq API

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Dieses Projekt ist ein Voice-to-Voice-Chatbot in Echtzeit, der OpenAIs Whisper für Speech-to-Text-Transkription nutzt, LLaMA 8B über die Groq API zur Antwortgenerierung verwendet und Google Text-to-Speech (gTTS) einsetzt, um Text wieder in Sprache umzuwandeln. Die Chatbot-Oberfläche ist mit Gradio erstellt, sodass Nutzer per Spracheingabe oder durch Hochladen von Audiodateien mit dem Bot interagieren können.

## Überblick

Die App implementiert einen vollständigen Audio-Konversationszyklus in einem einzigen Python-Skript:

1. Nutzeraudio vom Mikrofon oder aus einer hochgeladenen Datei entgegennehmen.
2. Sprache mit Whisper (Modell `base`) in Text transkribieren.
3. Eine Antwort über Groq (`llama3-8b-8192`) generieren.
4. Antworttext mit gTTS zurück in MP3-Audio umwandeln.
5. Sowohl Antworttext als auch abspielbares Audio in einer Gradio-Web-UI zurückgeben.

### Konversations-Pipeline

| Stufe | Komponente | Ausgabe |
|---|---|---|
| 🎙️ Eingabe | Gradio Audio (Mikrofon/Datei) | Audio-Pfad |
| 📝 Transkription | Whisper `base` | Nutzertext |
| 🧠 Verarbeitung | Groq + `llama3-8b-8192` | Assistententext |
| 🔊 Synthese | gTTS | MP3-Antwort |
| 🖥️ Ausgabe | Gradio UI | Text + abspielbares Audio |

## Funktionen

- **Speech-to-Text**: Wandelt gesprochene Sprache mit OpenAIs Whisper-Modell in Text um.
- **KI-generierte Antworten**: Verwendet LLaMA 8B über die Groq API, um auf Basis des transkribierten Textes intelligente Antworten zu erzeugen.
- **Text-to-Speech**: Wandelt generierte Textantworten mit Google Text-to-Speech (gTTS) wieder in Sprache um.
- **Echtzeit-Interaktion**: Der Chatbot arbeitet in Echtzeit und ermöglicht die Interaktion über ein Mikrofon oder per Upload von Audiodateien über eine Weboberfläche.
- **Einfacher Single-File-Betrieb**: Die gesamte Chatbot-Pipeline ist in `voice_to_voice_chatbot.py` implementiert und damit leicht zu testen.

## Projektstruktur

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # Main script to run the chatbot
├── requirements.txt                        # List of Python dependencies
├── i18n/                                   # Localized READMEs (planned/generated in pipeline)
└── .auto-readme-work/20260228_230442/      # README automation context/artifacts
```

Referenz der Legacy-Struktur aus der kanonischen README:

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # Main script to run the chatbot
├── requirements.txt           # List of Python dependencies
├── README.md                  # Project documentation
└── .gitignore                 # Git ignore file
```

## Voraussetzungen

Bevor du beginnst, stelle sicher, dass folgende Anforderungen erfüllt sind:

- Python 3.7 oder höher ist lokal oder in Google Colab installiert.
- Ein Groq API Key. Du kannst dich [hier](https://groq.com/) für einen API Key registrieren.
- [Google Colab](https://colab.research.google.com/) oder eine lokale Python-Umgebung mit den erforderlichen Bibliotheken.
- Internetzugang für:
  - den initialen Download des Whisper-Modells.
  - Groq API-Aufrufe.
  - gTTS-Audiogenerierung.

### Anforderungen auf einen Blick

| Anforderung | Warum sie benötigt wird |
|---|---|
| Python `3.7+` | Laufzeit für das Chatbot-Skript und seine Abhängigkeiten |
| Groq API Key | Authentifizierter Zugriff auf LLaMA-Modellinferenz |
| Colab oder lokale Umgebung | Ausführungsumgebung für Gradio + ML-Bibliotheken |
| Internetzugang | Whisper-Modell-Download, Groq-Anfragen, gTTS-Synthese |

## Installation

Führe diese Schritte aus, um das Projekt einzurichten:

1. **Repository klonen:**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Abhängigkeiten installieren:**

Installiere die erforderlichen Python-Bibliotheken:

```bash
pip install -r requirements.txt
```

Alternativ kannst du in Google Colab die Bibliotheken mit folgendem Befehl installieren:

```python
!pip install gradio groq-api openai-whisper gtts
```

## Konfiguration

### Groq API Key einrichten

Füge deinen Groq API Key zu den Umgebungsvariablen hinzu:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

In Google Colab kannst du den API Key so setzen:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Wichtiger Laufzeithinweis (aktuelles Code-Verhalten)

Das aktuelle Skript initialisiert den Groq-Client mit einem fest codierten Platzhalter:

```python
client = Groq(api_key="your_groq_api_key")
```

Wenn du nur `GROQ_API_KEY` in den Umgebungsvariablen setzt, musst du das Skript so anpassen, dass es aus `os.environ` liest, oder den Platzhalter direkt ersetzen, sonst schlägt die API-Authentifizierung fehl.

## Verwendung

Um den Chatbot zu starten, führe das Hauptskript aus:

```bash
python voice_to_voice_chatbot.py
```

Oder in Google Colab:

Kopiere das Skript in eine Codezelle und führe sie aus.

Die Gradio-Oberfläche startet und ermöglicht dir die Interaktion mit dem Chatbot.

### Interaktion mit dem Chatbot

- **Mikrofon verwenden**: Sprich direkt in das Mikrofon. Der Chatbot transkribiert deine Sprache, generiert eine Antwort und spielt sie als Audio ab.
- **Audio hochladen**: Lade eine zuvor aufgenommene Audiodatei hoch. Der Chatbot transkribiert das Audio, erzeugt eine Antwort und wandelt die Antwort wieder in Sprache um.

## Beispiele

### Beispielhafter Sprachfluss

1. Aufnahme: "What are three tips to learn Python quickly?"
2. Whisper transkribiert deinen Eingabetext.
3. Das Groq-LLaMA-Modell generiert eine Antwort.
4. gTTS erzeugt eine MP3-Antwort.
5. Gradio zeigt Antworttext und Audiowiedergabe an.

### Beispielhafter Ausführungsbefehl

```bash
python voice_to_voice_chatbot.py
```

Erwartetes Ergebnis: Eine lokale Gradio-App öffnet sich im Browser mit einem Audioeingang und zwei Ausgaben (Text + Audio).

## Entwicklungshinweise

- Hauptfunktion der Pipeline: `chatbot_pipeline(audio_path)`.
- Das Whisper-Modell wird beim Start geladen: `whisper.load_model("base")`.
- Temporäre MP3-Dateien werden über `NamedTemporaryFile(..., delete=False)` erstellt.
- Die Fehlerbehandlung gibt derzeit `(str(e), None)` an die UI zurück.
- Die Abhängigkeiten in `requirements.txt` enthalten sowohl `whisper` als auch `openai-whisper`; je nach Umgebung kann das redundant sein.

## Fehlerbehebung

### Häufige Probleme

`ModuleNotFoundError`: Stelle sicher, dass du die korrekte Version des Whisper-Moduls installiert hast mit

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: Prüfe deinen API Key und stelle sicher, dass er korrekt in den Umgebungsvariablen gesetzt ist.

Zusätzliche Fehlerbehebung:

- Wenn die App sofort mit Authentifizierungsfehlern fehlschlägt, prüfe den fest codierten Wert `api_key="your_groq_api_key"` in `voice_to_voice_chatbot.py`.
- Wenn keine Mikrofonaufnahme verfügbar ist, lade zuerst eine Audiodatei hoch, um die STT -> LLM -> TTS-Pipeline zu validieren.
- Wenn das Antwort-Audio leer ist, überprüfe den ausgehenden Netzwerkzugriff für gTTS.

### Schnelle Diagnose-Checkliste

| Prüfung | Validierung |
|---|---|
| API-Key-Anbindung | `Groq(api_key=...)` ist nicht als Platzhalter belassen |
| Whisper-Installation | `openai-whisper` lässt sich fehlerfrei importieren |
| Netzwerkpfad | Ausgehender Zugriff für Groq + gTTS verfügbar |
| Audioquelle | Mikrofonberechtigungen aktiv oder Dateiupload funktioniert |

## Roadmap

- Groq API Key standardmäßig direkt aus Umgebungsvariablen lesen.
- Tests für Pipeline-Hilfsfunktionen hinzufügen.
- Optionale Modell-/Konfigurationseinstellungen über Umgebungsvariablen oder CLI-Argumente hinzufügen.
- i18n-README-Dateien unter `i18n/` ergänzen, passend zu den Sprach-Navigationslinks.
- Deployment-Optionen hinzufügen (zum Beispiel Docker oder Hugging Face Spaces).

## Mitwirken

Beiträge sind willkommen. Bitte fork dieses Repository, erstelle einen neuen Branch und sende einen Pull Request mit deinen Änderungen.

Vorgeschlagener Beitragsablauf:

1. Repository forken und klonen.
2. Einen Feature-Branch erstellen.
3. Änderungen vornehmen und testen.
4. Pull Request mit klarer Beschreibung öffnen.

## Support

Im aktuellen Repository-Inhalt wurden keine expliziten Spenden-/Sponsoring-Links gefunden. Falls Maintainer Support-Kanäle ergänzen, sollten sie hier aufgeführt werden.

## Lizenz

Dieses Projekt ist unter der MIT License lizenziert. Siehe die LICENSE-Datei für Details.

Annahme: Eine `LICENSE`-Datei ist vorgesehen, aber in diesem Repository-Snapshot möglicherweise noch nicht vorhanden.
