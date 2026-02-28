[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Chatbot de voz a voz con Whisper, LLaMA y la API de Groq

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Este proyecto es un chatbot de voz a voz en tiempo real que aprovecha Whisper de OpenAI para la transcripción de voz a texto, LLaMA 8B mediante la API de Groq para generar respuestas, y Google Text-to-Speech (gTTS) para convertir texto nuevamente en voz. La interfaz del chatbot está construida con Gradio, lo que permite a los usuarios interactuar con el bot hablando o subiendo archivos de audio.

## Resumen

La app implementa un ciclo completo de conversación por audio en un solo script de Python:

1. Acepta audio del usuario desde micrófono o archivo subido.
2. Transcribe voz a texto con Whisper (modelo `base`).
3. Genera una respuesta mediante Groq (`llama3-8b-8192`).
4. Convierte el texto de la respuesta de vuelta a audio MP3 usando gTTS.
5. Devuelve tanto el texto de respuesta como el audio reproducible en una interfaz web de Gradio.

### Flujo de Conversación

| Etapa | Componente | Salida |
|---|---|---|
| 🎙️ Entrada | Gradio Audio (mic/file) | Ruta de audio |
| 📝 Transcripción | Whisper `base` | Texto del usuario |
| 🧠 Razonamiento | Groq + `llama3-8b-8192` | Texto del asistente |
| 🔊 Síntesis | gTTS | Respuesta MP3 |
| 🖥️ Entrega | Interfaz de Gradio | Texto + audio reproducible |

## Características

- **Voz a texto**: Convierte lenguaje hablado en texto usando el modelo Whisper de OpenAI.
- **Respuestas generadas por IA**: Usa LLaMA 8B mediante la API de Groq para generar respuestas inteligentes basadas en el texto transcrito.
- **Texto a voz**: Convierte las respuestas de texto generadas nuevamente en voz usando Google Text-to-Speech (gTTS).
- **Interacción en tiempo real**: El chatbot funciona en tiempo real, permitiendo a los usuarios interactuar mediante micrófono o subiendo archivos de audio a través de una interfaz web.
- **Ejecución simple en un solo archivo**: Toda la canalización del chatbot está implementada en `voice_to_voice_chatbot.py` para facilitar la experimentación.

## Estructura del Proyecto

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # Script principal para ejecutar el chatbot
├── requirements.txt                        # Lista de dependencias de Python
├── README.md                               # Documentación del proyecto (inglés)
├── i18n/                                   # READMEs localizados (planificados/generados en pipeline)
└── .auto-readme-work/20260228_230442/      # Contexto/artefactos de automatización del README
```

Referencia de estructura heredada del README canónico:

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # Script principal para ejecutar el chatbot
├── requirements.txt           # Lista de dependencias de Python
├── README.md                  # Documentación del proyecto
└── .gitignore                 # Archivo de exclusiones de Git
```

## Requisitos Previos

Antes de comenzar, asegúrate de cumplir con los siguientes requisitos:

- Python 3.7 o superior instalado en tu máquina local o en Google Colab.
- Una clave de API de Groq. Puedes registrarte para obtener una [aquí](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) o un entorno local de Python con las librerías necesarias instaladas.
- Acceso a internet para:
  - Descarga inicial del modelo Whisper.
  - Llamadas a la API de Groq.
  - Generación de audio con gTTS.

### Requisitos de un Vistazo

| Requisito | Por qué es necesario |
|---|---|
| Python `3.7+` | Entorno de ejecución para el script del chatbot y sus dependencias |
| Groq API Key | Acceso autenticado a la inferencia del modelo LLaMA |
| Colab or Local Env | Entorno de ejecución para Gradio + librerías de ML |
| Internet Access | Descarga del modelo Whisper, solicitudes a Groq, síntesis con gTTS |

## Instalación

Sigue estos pasos para configurar el proyecto:

1. **Clona el repositorio:**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Instala las dependencias:**

Instala las librerías de Python requeridas:

```bash
pip install -r requirements.txt
```

Como alternativa, si estás usando Google Colab, puedes instalar las librerías con:

```python
!pip install gradio groq-api openai-whisper gtts
```

## Configuración

### Configurar Groq API Key

Agrega tu clave de API de Groq a las variables de entorno:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

En Google Colab, puedes establecer la API key usando:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Nota Importante de Ejecución (Comportamiento actual del código)

El script actual inicializa el cliente de Groq con un marcador de posición codificado:

```python
client = Groq(api_key="your_groq_api_key")
```

Si solo configuras `GROQ_API_KEY` en el entorno, actualiza el script para leer desde `os.environ` o reemplaza directamente el marcador de posición; de lo contrario, fallará la autenticación con la API.

## Uso

Para iniciar el chatbot, ejecuta el script principal:

```bash
python voice_to_voice_chatbot.py
```

O en Google Colab:

Copia el script en una celda de código y ejecútalo.

La interfaz de Gradio se iniciará, permitiéndote interactuar con el chatbot.

### Interactuar con el Chatbot

- **Usando micrófono**: Habla directamente al micrófono. El chatbot transcribirá tu voz, generará una respuesta y la reproducirá como audio.
- **Subiendo audio**: Sube un archivo de audio pregrabado. El chatbot transcribirá el audio, generará una respuesta y volverá a convertirla en voz.

## Ejemplos

### Ejemplo de Flujo de Voz

1. Graba: "What are three tips to learn Python quickly?"
2. Whisper transcribe el texto de tu prompt.
3. El modelo LLaMA de Groq genera una respuesta.
4. gTTS produce una respuesta en MP3.
5. Gradio muestra el texto de respuesta y la reproducción de audio.

### Ejemplo de Comando de Ejecución

```bash
python voice_to_voice_chatbot.py
```

Resultado esperado: una app local de Gradio se abre en tu navegador con una entrada de audio y dos salidas (texto + audio).

## Notas de Desarrollo

- Función principal de la canalización: `chatbot_pipeline(audio_path)`.
- El modelo Whisper se carga al inicio: `whisper.load_model("base")`.
- Los archivos MP3 temporales se crean mediante `NamedTemporaryFile(..., delete=False)`.
- El manejo de errores actualmente devuelve `(str(e), None)` a la interfaz.
- Las dependencias en `requirements.txt` incluyen tanto `whisper` como `openai-whisper`; esto puede ser redundante según el entorno.

## Solución de Problemas

### Problemas Comunes

`ModuleNotFoundError`: Asegúrate de haber instalado la versión correcta del módulo Whisper con

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: Verifica de nuevo tu API key y asegúrate de que esté configurada correctamente en las variables de entorno.

Solución de problemas adicional:

- Si la app falla de inmediato con errores de autenticación, verifica el `api_key="your_groq_api_key"` codificado en `voice_to_voice_chatbot.py`.
- Si la captura por micrófono no está disponible, sube primero un archivo de audio para validar la canalización STT -> LLM -> TTS.
- Si el audio de respuesta está vacío, confirma el acceso de red saliente para gTTS.

### Lista Rápida de Diagnóstico

| Verificación | Validación |
|---|---|
| API key wiring | `Groq(api_key=...)` no se deja como marcador de posición |
| Whisper install | `openai-whisper` se importa correctamente |
| Network path | Hay acceso saliente disponible para Groq + gTTS |
| Audio source | Permisos de micrófono habilitados o carga de archivo funcional |

## Hoja de Ruta

- Leer Groq API key directamente desde variables de entorno de forma predeterminada.
- Añadir tests para funciones auxiliares de la canalización.
- Añadir configuración opcional de modelo mediante variables de entorno o argumentos CLI.
- Añadir archivos README i18n bajo `i18n/` que coincidan con los enlaces de navegación por idioma.
- Añadir opciones de despliegue (por ejemplo, Docker o Hugging Face Spaces).

## Contribuciones

Las contribuciones son bienvenidas. Haz un fork de este repositorio, crea una rama nueva y envía un pull request con tus cambios.

Flujo de contribución sugerido:

1. Haz fork y clona el repositorio.
2. Crea una rama de funcionalidad.
3. Realiza y prueba tus cambios.
4. Abre un pull request con una descripción clara.

## Soporte

No se encontraron enlaces explícitos de donación/patrocinio en el contenido actual del repositorio. Si los mantenedores añaden canales de soporte, deberían listarse aquí.

## Licencia

Este proyecto está licenciado bajo la licencia MIT. Consulta el archivo LICENSE para más detalles.

Suposición: se espera un archivo `LICENSE`, pero puede que aún no esté presente en esta instantánea del repositorio.
