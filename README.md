[English](README.md) · [العربية](i18n/README.ar.md) · [Español](i18n/README.es.md) · [Français](i18n/README.fr.md) · [日本語](i18n/README.ja.md) · [한국어](i18n/README.ko.md) · [Tiếng Việt](i18n/README.vi.md) · [中文 (简体)](i18n/README.zh-Hans.md) · [中文（繁體）](i18n/README.zh-Hant.md) · [Deutsch](i18n/README.de.md) · [Русский](i18n/README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Voice-to-Voice Chatbot using Whisper, LLaMA, and Groq API

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

This project is a real-time voice-to-voice chatbot that leverages OpenAI's Whisper for speech-to-text transcription, LLaMA 3 8B via the Groq API for response generation, and Google Text-to-Speech (gTTS) for converting text back into speech. The interface uses Gradio so users can interact by speaking or uploading audio files.

## Overview

The app implements a full audio conversation loop in one Python script:

1. Accept user audio from a microphone or uploaded file.
2. Transcribe speech to text with Whisper (`base`) model.
3. Generate a response via Groq (`llama3-8b-8192`).
4. Convert response text back to MP3 audio using gTTS.
5. Return both response text and playable audio in a Gradio web UI.

### Conversation Pipeline

| Stage | Component | Output |
|---|---|---|
| 🎙️ Input | Gradio Audio (mic/file) | Audio path |
| 📝 Transcription | Whisper `base` | User text |
| 🧠 Reasoning | Groq + `llama3-8b-8192` | Assistant text |
| 🔊 Synthesis | gTTS | MP3 response |
| 🖥️ Delivery | Gradio UI | Text + playable audio |

## ⭐ Features

- **Speech-to-Text**: Convert spoken language into text using OpenAI's Whisper model.
- **AI-Generated Responses**: Use LLaMA 8B via the Groq API to generate intelligent responses based on transcribed text.
- **Text-to-Speech**: Convert response text back into speech using Google Text-to-Speech (gTTS).
- **Real-Time Interaction**: Interact through a microphone or by uploading audio files via the web interface.
- **Simple Single-File Runtime**: The complete chatbot pipeline is implemented in `voice_to_voice_chatbot.py`.
- **Multilingual Documentation**: Primary README is mirrored by links to language-specific translations in `i18n/`.

## 📁 Project Structure

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

## ✅ Prerequisites

Before you begin, ensure you have met the following requirements:

- Python 3.7 or higher installed on your local machine or Google Colab.
- A Groq API key. You can sign up for an API key at [Groq](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) or a local Python environment with required libraries.
- Internet access for:
  - Initial Whisper model download.
  - Groq API calls.
  - gTTS audio generation.

### Requirements at a Glance

| Requirement | Why It Is Needed |
|---|---|
| Python `3.7+` | Runtime for the chatbot script and dependencies |
| Groq API Key | Authenticated access to LLaMA model inference |
| Colab or Local Env | Execution environment for Gradio + ML libraries |
| Internet Access | Whisper model download, Groq requests, gTTS synthesis |

## 🛠️ Installation

Follow these steps to set up the project:

1. **Clone the Repository**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Install Dependencies**

Install the required Python libraries:

```bash
pip install -r requirements.txt
```

Alternatively, in Google Colab:

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ Configuration

### Set Up Groq API Key

Export your Groq API key:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

In Google Colab, set it in runtime:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Important Runtime Note (Current Code Behavior)

The current script initializes the Groq client with a hardcoded placeholder:

```python
client = Groq(api_key="your_groq_api_key")
```

If you only set `GROQ_API_KEY` in the environment, update the script to read from `os.environ` (or replace the placeholder directly), otherwise authentication calls can fail.

## ▶️ Usage

To start the chatbot, run:

```bash
python voice_to_voice_chatbot.py
```

Or in Google Colab:

Copy the script into a code cell and execute it.

The Gradio interface will launch locally, allowing you to interact with the chatbot.

### Interacting with the Chatbot

- **Using Microphone**: Speak directly into the microphone. The chatbot transcribes your speech, generates a response, and plays it back as audio.
- **Uploading Audio**: Upload a pre-recorded audio file. The chatbot transcribes the recording, generates a response, and plays the synthesized audio.

## 🎬 Examples

### Example Voice Flow

1. Record: "What are three tips to learn Python quickly?"
2. Whisper transcribes your prompt text.
3. Groq LLaMA model generates an answer.
4. gTTS produces an MP3 response.
5. Gradio displays response text and an audio player.

### Example Run Command

```bash
python voice_to_voice_chatbot.py
```

Expected result: a local Gradio app opens in your browser with one audio input and two outputs (text + audio).

## 🧪 Development Notes

- Main pipeline function: `chatbot_pipeline(audio_path)`.
- Whisper model is loaded at startup with `whisper.load_model("base")`.
- Temporary MP3 files are created using `NamedTemporaryFile(..., delete=False)`.
- Error handling currently returns `(str(e), None)` to the UI.
- `requirements.txt` includes both `whisper` and `openai-whisper`; this may be redundant depending on environment.

## 🐞 Troubleshooting

### Common Issues

`ModuleNotFoundError`: Ensure the correct Whisper package is installed:

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: Verify key availability and validity in your environment or script.

Additional troubleshooting:

- If the app fails with authentication errors, verify the hardcoded `api_key="your_groq_api_key"` in `voice_to_voice_chatbot.py`.
- If microphone capture is unavailable, upload an audio file first to validate the STT → LLM → TTS pipeline.
- If response audio is empty, confirm outbound network access for gTTS and Groq.

### Quick Diagnostics Checklist

| Check | Validation |
|---|---|
| API key wiring | `Groq(api_key=...)` is not left as placeholder |
| Whisper install | `openai-whisper` imports cleanly |
| Network path | Outbound access available for Groq + gTTS |
| Audio source | Mic permissions enabled or file upload works |

## 🗺️ Roadmap

- Read Groq API key directly from environment variables by default.
- Add tests for pipeline helper functions.
- Add optional model/config settings via environment variables or CLI args.
- Add deployment options (for example, Docker or Hugging Face Spaces).

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 Contributing

Contributions are welcome. Please fork this repository, create a new branch, and submit a pull request with your changes.

Suggested contribution flow:

1. Fork and clone the repository.
2. Create a feature branch.
3. Implement and test your changes.
4. Open a pull request with a clear description and rationale.

## 📄 License

This project is currently documented as MIT licensed. See the LICENSE file for details.

Assumption: a `LICENSE` file is intended but may not yet be present in this repository snapshot.
