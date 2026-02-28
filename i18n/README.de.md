[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Voice-to-Voice-Chatbot mit Whisper, LLaMA und Groq API

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Dieses Projekt ist ein Echtzeit-Voice-to-Voice-Chatbot, der OpenAIs Whisper für die Sprache-zu-Text-Transkription nutzt, Groq API mit LLaMA 3 8B zur Generierung von Antworten verwendet und Google Text-to-Speech (gTTS) für die Umwandlung von Text zurück in Sprache einsetzt. Die Oberfläche basiert auf Gradio, sodass Nutzer:innen per Spracheingabe oder durch Hochladen von Audiodateien interagieren können.

## Überblick

Die App implementiert einen vollständigen Audio-Konversationsablauf in einem einzelnen Python-Skript:

1. Erfasst Audiodaten aus Mikrofon oder hochgeladener Datei.
2. Transkribiert Sprache mit dem Whisper-Modell (`base`) in Text.
3. Generiert eine Antwort über Groq (`llama3-8b-8192`).
4. Wandelt Antworttext mit gTTS in MP3-Audio um.
5. Gibt sowohl Antworttext als auch abspielbares Audio in einer Gradio-Weboberfläche zurück.

### Konversations-Pipeline

| Stufe | Komponente | Ausgabe |
|---|---|---|
| 🎙️ Eingabe | Gradio Audio (Mikrofon/Datei) | Audio-Pfad |
| 📝 Transkription | Whisper `base` | Nutzertext |
| 🧠 Verarbeitung | Groq + `llama3-8b-8192` | Assistententext |
| 🔊 Synthese | gTTS | MP3-Antwort |
| 🖥️ Bereitstellung | Gradio UI | Text + abspielbares Audio |

## ⭐ Funktionen

- **Speech-to-Text**: Wandelt gesprochene Sprache mit dem Whisper-Modell von OpenAI in Text um.
- **KI-generierte Antworten**: Nutzt LLaMA 8B über die Groq API, um auf Basis transkribierten Texts intelligente Antworten zu erzeugen.
- **Text-to-Speech**: Wandelt Antworttext mit Google Text-to-Speech (gTTS) zurück in Sprache um.
- **Echtzeit-Interaktion**: Interaktion über Mikrofon oder Upload von Audiodateien direkt über die Weboberfläche.
- **Einfacher Single-File-Betrieb**: Die vollständige Chatbot-Pipeline ist in `voice_to_voice_chatbot.py` implementiert.
- **Mehrsprachige Dokumentation**: In der Haupt-README verweisen Links auf Sprachversionen in `i18n/`.

## 📁 Projektstruktur

```text
Voice-to-text-and-voice-chatbot/
├── requirements.txt              # Python dependencies
├── voice_to_voice_chatbot.py     # Main script to run the chatbot
├── i18n/                        # Localized READMEs
│   ├── README.ar.md
│   ├── README.de.md
│   ├── README.es.md
│   ├── README.fr.md
│   ├── README.ja.md
│   ├── README.ko.md
│   ├── README.ru.md
│   ├── README.vi.md
│   ├── README.zh-Hans.md
│   └── README.zh-Hant.md
└── .auto-readme-work/
    ├── 20260228_230442/
    └── 20260301_064403/
```

## ✅ Voraussetzungen

Bevor Sie beginnen, stellen Sie sicher, dass Sie die folgenden Anforderungen erfüllt haben:

- Python 3.7 oder höher auf Ihrem lokalen Rechner oder Google Colab installiert.
- Ein Groq API-Schlüssel. Sie können sich für einen API-Schlüssel bei [Groq](https://groq.com/) anmelden.
- [Google Colab](https://colab.research.google.com/) oder eine lokale Python-Umgebung mit den erforderlichen Bibliotheken.
- Internetzugang für:
  - initialer Download des Whisper-Modells.
  - Groq API-Aufrufe.
  - gTTS Audioerzeugung.

### Anforderungen auf einen Blick

| Anforderung | Warum es nötig ist |
|---|---|
| Python `3.7+` | Laufzeit für das Chatbot-Skript und Abhängigkeiten |
| Groq API Key | Authentifizierter Zugriff auf LLaMA-Modell-Inferenz |
| Colab oder lokale Umgebung | Ausführungsumgebung für Gradio + ML-Bibliotheken |
| Internetzugang | Whisper-Modell-Download, Groq-Anfragen, gTTS-Synthese |

## 🛠️ Installation

Führen Sie diese Schritte aus, um das Projekt einzurichten:

1. **Repository klonen**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Abhängigkeiten installieren**

Installieren Sie die benötigten Python-Bibliotheken:

```bash
pip install -r requirements.txt
```

Alternativ in Google Colab:

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ Konfiguration

### Groq API-Schlüssel einrichten

Exportieren Sie Ihren Groq API-Schlüssel:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

In Google Colab setzen Sie ihn in der Laufzeit:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Wichtiger Laufzeit-Hinweis (aktuelles Code-Verhalten)

Das aktuelle Skript initialisiert den Groq-Client mit einem hartkodierten Platzhalter:

```python
client = Groq(api_key="your_groq_api_key")
```

Wenn Sie `GROQ_API_KEY` nur in der Umgebung gesetzt haben, aktualisieren Sie das Skript, damit es aus `os.environ` liest (oder ersetzen Sie den Platzhalter direkt), andernfalls können Authentifizierungsaufrufe fehlschlagen.

## ▶️ Verwendung

Starten Sie den Chatbot mit:

```bash
python voice_to_voice_chatbot.py
```

Oder in Google Colab:

Kopieren Sie das Skript in eine Code-Zelle und führen Sie es aus.

Die Gradio-Oberfläche startet lokal und ermöglicht die Interaktion mit dem Chatbot.

### Interaktion mit dem Chatbot

- **Mikrofon verwenden**: Sprechen Sie direkt in das Mikrofon. Der Chatbot transkribiert Ihre Sprache, erzeugt eine Antwort und spielt sie als Audio ab.
- **Audio hochladen**: Laden Sie eine voraufgezeichnete Audiodatei hoch. Der Chatbot transkribiert die Aufnahme, erzeugt eine Antwort und spielt das synthetisierte Audio ab.

## 🎬 Beispiele

### Beispielhafter Sprachablauf

1. Aufnahme: "What are three tips to learn Python quickly?"
2. Whisper transkribiert Ihren Eingabetext.
3. Das Groq-LLaMA-Modell erzeugt eine Antwort.
4. gTTS erstellt eine MP3-Antwort.
5. Gradio zeigt Antworttext und einen Audio-Player an.

### Beispielhafter Ausführungsbefehl

```bash
python voice_to_voice_chatbot.py
```

Erwartetes Ergebnis: Eine lokale Gradio-App öffnet sich in Ihrem Browser mit einem Audioeingang und zwei Ausgaben (Text + Audio).

## 🧪 Entwicklungshinweise

- Haupt-Pipeline-Funktion: `chatbot_pipeline(audio_path)`.
- Das Whisper-Modell wird beim Start mit `whisper.load_model("base")` geladen.
- Temporäre MP3-Dateien werden mit `NamedTemporaryFile(..., delete=False)` erstellt.
- Fehlerbehandlung gibt aktuell `(str(e), None)` an die UI zurück.
- `requirements.txt` enthält sowohl `whisper` als auch `openai-whisper`; je nach Umgebung kann das redundant sein.

## 🐞 Fehlerbehebung

### Häufige Probleme

`ModuleNotFoundError`: Stellen Sie sicher, dass das richtige Whisper-Paket installiert ist:

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: Überprüfen Sie die Verfügbarkeit und Gültigkeit Ihres Schlüssels in Umgebung oder Skript.

Zusätzliche Fehlerbehebung:

- Wenn die App mit Authentifizierungsfehlern endet, prüfen Sie den hartkodierten Wert `api_key="your_groq_api_key"` in `voice_to_voice_chatbot.py`.
- Wenn keine Mikrofonaufnahme verfügbar ist, laden Sie zuerst eine Audiodatei hoch, um die STT → LLM → TTS-Pipeline zu validieren.
- Wenn das Antwort-Audio leer ist, prüfen Sie den ausgehenden Netzwerkzugriff für gTTS und Groq.

### Schnelle Diagnose-Checkliste

| Prüfung | Validierung |
|---|---|
| API-Key-Konfiguration | `Groq(api_key=...)` bleibt nicht als Platzhalter stehen |
| Whisper-Installation | `openai-whisper` lässt sich sauber importieren |
| Netzwerkpfad | Ausgehender Zugriff für Groq + gTTS verfügbar |
| Audioquelle | Mikrofonberechtigungen aktiviert oder Dateiupload funktioniert |

## 🗺️ Roadmap

- Groq API-Schlüssel standardmäßig direkt aus Umgebungsvariablen lesen.
- Tests für Pipeline-Hilfsfunktionen hinzufügen.
- Optionale Modell- und Konfigurationseinstellungen über Umgebungsvariablen oder CLI-Argumente hinzufügen.
- Bereitstellungsoptionen hinzufügen (zum Beispiel Docker oder Hugging Face Spaces).

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 Mitwirken

Beiträge sind willkommen. Bitte forken Sie dieses Repository, erstellen Sie einen neuen Branch und senden Sie einen Pull Request mit Ihren Änderungen.

Vorgeschlagener Beitragsablauf:

1. Repository forken und klonen.
2. Einen Feature-Branch erstellen.
3. Änderungen implementieren und testen.
4. Einen Pull Request mit klarer Beschreibung und Begründung öffnen.

## 📄 Lizenz

Dieses Projekt ist derzeit als MIT-lizenziert dokumentiert. Siehe die LICENSE-Datei für Details.

Annahme: Eine `LICENSE`-Datei ist vorgesehen, könnte im aktuellen Repository-Snapshot jedoch noch nicht vorhanden sein.
