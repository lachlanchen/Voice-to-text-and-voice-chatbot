[English](README.md) · [العربية](i18n/README.ar.md) · [Español](i18n/README.es.md) · [Français](i18n/README.fr.md) · [日本語](i18n/README.ja.md) · [한국어](i18n/README.ko.md) · [Tiếng Việt](i18n/README.vi.md) · [中文 (简体)](i18n/README.zh-Hans.md) · [中文（繁體）](i18n/README.zh-Hant.md) · [Deutsch](i18n/README.de.md) · [Русский](i18n/README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Voice-to-Voice Chatbot using Whisper, LLaMA, and Groq API

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20Text-to-Speech-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)
![Platform](https://img.shields.io/badge/Runtime-Local%20%2F%20Colab-0EA5E9?logo=jupyter&logoColor=white)

This repository provides a compact, single-file voice chatbot. It captures speech, transcribes it with Whisper, sends the text to Groq-hosted LLaMA for reasoning, and synthesizes a spoken answer through Google Text-to-Speech (gTTS). The end-user interaction is handled by Gradio with both text and audio outputs.

> **Goal:** a practical, reproducible pipeline you can run locally or in Colab with a single main script.

## 🧭 Quick Snapshot

| Area | Status |
|---|---|
| Language scope | `README.md` plus multilingual copies in `i18n/` |
| Recommended run mode | `Local` first, `Colab` second |

## 🔎 Quick Snapshot Details

| Focus | Status |
|---|---|
| Entry point | `voice_to_voice_chatbot.py` |
| Interface | Gradio-based web UI with text + audio |
| STT model | Whisper (`base`) |
| LLM backend | Groq-hosted `llama3-8b-8192` |
| TTS engine | Google Text-to-Speech |
| Language docs | 10+ translated README files in `i18n/` |

## Overview

The app implements an end-to-end conversation pipeline in `voice_to_voice_chatbot.py`:

1. Receive user audio from a microphone input or file upload.
2. Transcribe speech to text using Whisper (`base`) model.
3. Generate a response using Groq and `llama3-8b-8192`.
4. Convert generated text into MP3 with gTTS.
5. Render the text response and playback controls in Gradio.

### Conversation Pipeline

| Stage | Component | Output |
|---|---|---|
| 🎙️ Input | `gr.Audio(type="filepath")` | Audio file path |
| 📝 Transcription | Whisper `base` model | Transcript text |
| 🧠 Reasoning | Groq chat completion | Assistant response text |
| 🔊 Synthesis | `gTTS` | MP3 response path |
| 🖥️ Delivery | Gradio `Interface` | Response text + audio playback |

```mermaid
flowchart LR
    A[Audio Input] --> B[Whisper Transcription]
    B --> C[Groq LLaMA Reasoning]
    C --> D[gTTS Synthesis]
    D --> E[Gradio Output: Text + MP3]
```

## ⭐ Features

- **STT + LLM + TTS in one script**: full voice loop in `voice_to_voice_chatbot.py`.
- **Microphone and file support**: pick live speech or upload recorded files.
- **Lightweight setup**: only a small set of Python packages.
- **Multilingual documentation**: localized READMEs are maintained under `i18n/`.
- **Practical debugging visibility**: function-level error returns surface in UI for fast iteration.

## 📁 Project Structure

```text
Voice-to-text-and-voice-chatbot/
    ├── requirements.txt              # Python dependencies
    ├── voice_to_voice_chatbot.py     # Main application script
    ├── i18n/                        # Translated README files
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
└── .auto-readme-work/            # Metadata produced for README generation
    ├── 20260228_230442/
    ├── 20260301_064403/
    └── 20260301_065134/
        ├── language-nav-i18n.md
        ├── language-nav-root.md
        ├── pipeline-context.md
        └── translation-plan.txt
```

## 🌍 Localization and Documentation

This README project keeps one source of truth in English and provides translated variants in `i18n/`.

- Use the language links near the top of this file to switch between translated READMEs.
- Existing translations cover 10+ locales and should be kept in sync with the English structure.
- Prefer updating the English README first, then align translated files with major structure and command changes.

## ✅ Prerequisites

- Python 3.7+ runtime.
- A valid Groq API key.
- Internet access for Whisper model download and API calls.
- Optional: microphone permissions in the browser if you use live audio.
- Optional: a GPU can improve Whisper transcription latency and consistency.

### Requirements at a Glance

| Requirement | Why it is needed |
|---|---|
| Python `3.7+` | Runtime for Gradio, Whisper, and dependencies |
| Groq API key | Required to call LLM inference |
| `requirements.txt` | Installs all required Python packages |
| Browser mic access | Enables voice input through Gradio |

## 🛠️ Installation

1. Clone the repository:

```bash
git clone <repo-url>
cd Voice-to-text-and-voice-chatbot
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

For Google Colab use:

```python
!pip install -U gradio openai-whisper gtts groq
```

### Notes

- The repo currently declares both `whisper` and `openai-whisper` in requirements.
- If you encounter package conflicts, prefer the variant that matches your environment and remove redundant installs once validated.

## 🧯 Run-Readiness Checklist

| Step | Check |
|---|---|
| API key | `GROQ_API_KEY` or trusted local fallback is correctly configured |
| Audio device | Browser microphone is enabled for live input |
| Runtime path | Commands run from project root with dependencies installed |
| Output path | Temp directories are writable for MP3 responses |

## ⚙️ Configuration

### Environment variable (recommended)

```bash
export GROQ_API_KEY='your_groq_api_key'
```

In Colab runtime:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Important runtime note (current behavior)

`voice_to_voice_chatbot.py` currently initializes Groq as:

```python
client = Groq(
    api_key="your_groq_api_key",
)
```

If you only set `GROQ_API_KEY`, update the script to read from `os.getenv` or hardcode from a trusted local environment variable before running:

```python
client = Groq(api_key=os.getenv("GROQ_API_KEY", "your_groq_api_key"))
```

### Assumptions

- This repo is expected to be run in a local Python environment or Colab.
- No separate server entrypoint or deployment config is present in this snapshot.

## ▶️ Usage

Start the application with:

```bash
python voice_to_voice_chatbot.py
```

Gradio will launch a local interface with one audio input and two outputs:

- `Response Text`
- `Response Audio`

### Interacting with the Chatbot

- **Microphone**: click record and speak; the audio is transcribed, answered, then played back.
- **Upload file**: choose an audio file for transcription and response generation.

## 🎬 Examples

### Example flow

1. Ask: "What are three tips to learn Python quickly?"
2. Whisper returns a transcript.
3. Groq generates an answer.
4. gTTS synthesizes the output.
5. UI displays the text and audio reply.

### Expected output

- Successful transcription shown in the response text box.
- Non-empty spoken response file in the Gradio audio player.

## 🧪 Development Notes

- Core function: `chatbot_pipeline(audio_path)`.
- Whisper is loaded once at module import with `whisper.load_model("base")`.
- Audio output uses `NamedTemporaryFile(..., delete=False)` for mp3 persistence.
- Error path returns `(str(e), None)` to keep UI responsive on failures.
- `iface.launch()` is invoked at module import; for library-style usage, consider guarding launch code with `if __name__ == "__main__":`.

## 🐞 Troubleshooting

### Common issues

- `ModuleNotFoundError` for Whisper:

```bash
pip install -U openai-whisper
```

- Groq auth failures:
  - Ensure the placeholder API key is replaced or loaded from env.
  - Confirm the key has sufficient permissions and quota.

- No output audio:
  - Check outbound connectivity for Groq and gTTS.
  - Ensure temp MP3 path is writable in the environment.

### Quick diagnostics checklist

| Check | Validation |
|---|---|
| API key source | `Groq(api_key=...)` is a valid key |
| STT dependency | `import whisper` and `openai-whisper` import succeed |
| Audio path | Gradio receives a valid audio filepath |
| Output render | The UI returns both response text and audio |

## 🗺️ Roadmap

- Replace hardcoded Groq key with fully env-driven configuration by default.
- Add environment-based model selection (`whisper` size, Groq model ID).
- Add minimal tests for helper functions.
- Add CLI and deployment presets (Docker/Hugging Face Spaces).

## ♻️ Maintenance and Sync Strategy

To keep multilingual README quality consistent:

2. Mirror headings and key content updates into `i18n/` translations.
3. Keep banner and support blocks aligned across localized versions.

## 🤝 Contributing

Contributions are welcome. Suggested flow:

1. Fork the repository.
2. Create a feature branch.
3. Implement your changes.
4. Open a clear pull request with rationale and testing notes.

## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 📄 License

This repository references MIT licensing intent, but no `LICENSE` file is present in this snapshot. Please add a license file if licensing is expected for distribution.
