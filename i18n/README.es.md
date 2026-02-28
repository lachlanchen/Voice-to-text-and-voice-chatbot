[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Chatbot de voz a voz con Whisper, LLaMA y la API de Groq

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Este proyecto es un chatbot de voz a voz en tiempo real que utiliza Whisper de OpenAI para la transcripción de voz a texto, LLaMA 3 8B mediante la API de Groq para generar respuestas y Google Text-to-Speech (gTTS) para convertir texto de nuevo en voz. La interfaz usa Gradio para que los usuarios interactúen hablando o subiendo archivos de audio.

## Overview

La aplicación implementa un bucle completo de conversación por audio en un único script de Python:

1. Acepta audio del usuario desde un micrófono o un archivo subido.
2. Transcribe el habla a texto con el modelo Whisper (`base`).
3. Genera una respuesta mediante Groq (`llama3-8b-8192`).
4. Convierte el texto de la respuesta a MP3 con gTTS.
5. Devuelve tanto el texto de respuesta como el audio reproducible en una interfaz web de Gradio.

### Conversation Pipeline

| Etapa | Componente | Salida |
|---|---|---|
| 🎙️ Entrada | Gradio Audio (mic/file) | Ruta de audio |
| 📝 Transcripción | Whisper `base` | Texto del usuario |
| 🧠 Razonamiento | Groq + `llama3-8b-8192` | Texto del asistente |
| 🔊 Síntesis | gTTS | Respuesta MP3 |
| 🖥️ Entrega | Gradio UI | Texto + audio reproducible |

## ⭐ Features

- **Speech-to-Text**: Convierte lenguaje hablado a texto usando el modelo Whisper de OpenAI.
- **Respuestas generadas por IA**: Usa LLaMA 8B vía la API de Groq para generar respuestas inteligentes basadas en el texto transcrito.
- **Text-to-Speech**: Convierte el texto de respuesta a voz con Google Text-to-Speech (gTTS).
- **Interacción en tiempo real**: Interactúa mediante micrófono o subiendo archivos de audio desde la interfaz web.
- **Runtime simple de un solo archivo**: El pipeline completo del chatbot está implementado en `voice_to_voice_chatbot.py`.
- **Documentación multilingüe**: El README principal enlaza versiones traducidas en `i18n/`.

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

Antes de empezar, asegúrate de cumplir con estos requisitos:

- Python 3.7 o superior instalado en tu máquina local o en Google Colab.
- Una clave de API de Groq. Puedes obtenerla en [Groq](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) o un entorno Python local con las librerías necesarias.
- Acceso a Internet para:
  - Descarga inicial del modelo Whisper.
  - Llamadas a la API de Groq.
  - Generación de audio con gTTS.

### Requirements at a Glance

| Requisito | ¿Por qué se necesita? |
|---|---|
| Python `3.7+` | Entorno de ejecución para el script del chatbot y sus dependencias |
| Groq API Key | Acceso autenticado a la inferencia del modelo LLaMA |
| Colab o entorno local | Entorno de ejecución para Gradio + librerías de ML |
| Acceso a Internet | Descarga del modelo Whisper, solicitudes a Groq y síntesis con gTTS |

## 🛠️ Installation

Sigue estos pasos para configurar el proyecto:

1. **Clona el repositorio**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Instala dependencias**

Instala las librerías necesarias:

```bash
pip install -r requirements.txt
```

Como alternativa, en Google Colab:

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ Configuration

### Configura la clave de Groq

Exporta tu clave de API de Groq:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

En Google Colab, configúrala en tiempo de ejecución:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Nota importante de ejecución (comportamiento actual del código)

El script actual inicializa el cliente de Groq con un marcador de posición fijo:

```python
client = Groq(api_key="your_groq_api_key")
```

Si solo estableces `GROQ_API_KEY` en el entorno, actualiza el script para que lea `os.environ` (o reemplaza el marcador de posición directamente), de lo contrario las llamadas de autenticación pueden fallar.

## ▶️ Usage

Para iniciar el chatbot, ejecuta:

```bash
python voice_to_voice_chatbot.py
```

O en Google Colab:

Copia el script en una celda de código y ejecútalo.

La interfaz de Gradio se abrirá localmente para que puedas interactuar con el chatbot.

### Interacting with the Chatbot

- **Usar micrófono**: Habla directamente en el micrófono. El chatbot transcribe tu voz, genera una respuesta y la reproduce como audio.
- **Subir audio**: Sube un archivo de audio pregrabado. El chatbot transcribe la grabación, genera una respuesta y reproduce el audio sintetizado.

## 🎬 Examples

### Example Voice Flow

1. Graba: "What are three tips to learn Python quickly?"
2. Whisper transcribe tu prompt.
3. Groq LLaMA genera una respuesta.
4. gTTS produce una respuesta MP3.
5. Gradio muestra el texto de respuesta y un reproductor de audio.

### Example Run Command

```bash
python voice_to_voice_chatbot.py
```

Resultado esperado: se abrirá una app local de Gradio en tu navegador con una entrada de audio y dos salidas (texto + audio).

## 🧪 Development Notes

- Función principal del pipeline: `chatbot_pipeline(audio_path)`.
- El modelo Whisper se carga al inicio con `whisper.load_model("base")`.
- Los archivos MP3 temporales se crean con `NamedTemporaryFile(..., delete=False)`.
- El manejo de errores actualmente devuelve `(str(e), None)` a la UI.
- `requirements.txt` incluye `whisper` y `openai-whisper`; puede ser redundante según el entorno.

## 🐞 Troubleshooting

### Common Issues

`ModuleNotFoundError`: Asegúrate de tener el paquete Whisper correcto instalado:

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: Verifica que la clave exista y sea válida en tu entorno o script.

Solución adicional:

- Si la app falla con errores de autenticación, verifica el `api_key="your_groq_api_key"` fijo en `voice_to_voice_chatbot.py`.
- Si la captura de micrófono no está disponible, primero sube un archivo de audio para validar el pipeline STT → LLM → TTS.
- Si el audio de respuesta está vacío, confirma el acceso a red saliente para gTTS y Groq.

### Quick Diagnostics Checklist

| Verificación | Validación |
|---|---|
| Configuración de la clave API | `Groq(api_key=...)` no queda como marcador de posición |
| Instalación de Whisper | `openai-whisper` se importa correctamente |
| Ruta de red | Hay acceso saliente disponible para Groq + gTTS |
| Fuente de audio | Permisos de micrófono habilitados o carga de archivo funcional |

## 🗺️ Roadmap

- Leer la clave API de Groq directamente de variables de entorno de forma predeterminada.
- Añadir pruebas para funciones auxiliares del pipeline.
- Añadir configuración opcional de modelo/configuración mediante variables de entorno o argumentos CLI.
- Añadir opciones de despliegue (por ejemplo, Docker o Hugging Face Spaces).

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 Contributing

Las contribuciones son bienvenidas. Haz un fork de este repositorio, crea una rama nueva y envía un pull request con tus cambios.

Flujo de contribución sugerido:

1. Haz fork y clona el repositorio.
2. Crea una rama para tu funcionalidad.
3. Implementa y prueba tus cambios.
4. Abre un pull request con una descripción y una justificación claras.

## 📄 License

Este proyecto se documenta actualmente bajo la licencia MIT. Consulta el archivo LICENSE para más detalles.

Se asume que debe existir un archivo `LICENSE`, aunque puede que aún no esté presente en esta instantánea del repositorio.
