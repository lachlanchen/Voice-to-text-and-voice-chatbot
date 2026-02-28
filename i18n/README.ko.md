[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Whisper, LLaMA, Groq API를 사용한 음성-음성 챗봇

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

이 프로젝트는 OpenAI Whisper로 음성 인식을 처리하고, Groq API 기반의 LLaMA 3 8B로 응답을 생성한 뒤, Google Text-to-Speech(gTTS)로 다시 음성으로 변환하는 실시간 음성-음성 챗봇입니다. Gradio UI를 통해 사용자는 음성 입력 또는 오디오 파일 업로드 방식으로 대화할 수 있습니다.

## 개요

이 앱은 하나의 Python 스크립트로 전체 오디오 대화 루프를 구현합니다:

1. 마이크 또는 업로드된 파일에서 사용자 오디오를 수신합니다.
2. Whisper(`base`) 모델로 음성을 텍스트로 전사합니다.
3. Groq(`llama3-8b-8192`)를 사용해 응답을 생성합니다.
4. gTTS로 응답 텍스트를 MP3 오디오로 변환합니다.
5. Gradio 웹 UI에서 텍스트 응답과 재생 가능한 오디오를 모두 반환합니다.

### 대화 파이프라인

| 단계 | 구성 요소 | 출력 |
|---|---|---|
| 🎙️ 입력 | Gradio Audio (mic/file) | 오디오 경로 |
| 📝 전사 | Whisper `base` | 사용자 텍스트 |
| 🧠 추론 | Groq + `llama3-8b-8192` | 어시스턴트 텍스트 |
| 🔊 합성 | gTTS | MP3 응답 |
| 🖥️ 전달 | Gradio UI | 텍스트 + 재생 가능한 오디오 |

## ⭐ 기능

- **Speech-to-Text**: OpenAI의 Whisper 모델을 사용해 음성을 텍스트로 변환합니다.
- **AI-Generated Responses**: 전사된 텍스트를 기반으로 Groq API의 LLaMA 8B가 지능형 응답을 생성합니다.
- **Text-to-Speech**: 생성된 텍스트 응답을 Google Text-to-Speech(gTTS)로 다시 음성으로 변환합니다.
- **실시간 상호작용**: 웹 인터페이스에서 마이크를 사용하거나 오디오 파일을 업로드해 실시간으로 상호작용할 수 있습니다.
- **단일 파일 실행**: 전체 챗봇 파이프라인이 `voice_to_voice_chatbot.py` 하나의 스크립트에 구현되어 있습니다.
- **다국어 문서화**: `i18n/`에 언어별 README 링크가 포함되어 있어 주 README를 여러 언어로 제공합니다.

## 📁 프로젝트 구조

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

## ✅ 선행 조건

시작하기 전에 다음 요구사항이 충족되었는지 확인하세요:

- 로컬 머신 또는 Google Colab에 Python 3.7 이상이 설치되어 있어야 합니다.
- Groq API 키가 있어야 합니다. Groq에서 [API 키를 발급받을 수 있습니다](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) 또는 필요한 라이브러리가 설치된 로컬 Python 환경.
- 다음 용도를 위한 인터넷 연결:
  - 초기 Whisper 모델 다운로드
  - Groq API 호출
  - gTTS 오디오 생성

### 요구사항 한눈에 보기

| 요구사항 | 필요 이유 |
|---|---|
| Python `3.7+` | 챗봇 스크립트 및 의존성 실행용 런타임 |
| Groq API Key | LLaMA 모델 추론에 필요한 인증된 접근 |
| Colab 또는 로컬 환경 | Gradio + ML 라이브러리를 실행할 환경 |
| 인터넷 접근 | Whisper 모델 다운로드, Groq 요청, gTTS 합성 |

## 🛠️ 설치

다음 단계에 따라 프로젝트를 설정하세요:

1. **저장소 클론**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **의존성 설치**

필요한 Python 라이브러리를 설치합니다:

```bash
pip install -r requirements.txt
```

Google Colab에서는 다음처럼 설치할 수 있습니다:

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ 구성

### Groq API 키 설정

Groq API 키를 환경 변수에 추가합니다:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Google Colab에서는 런타임에서 다음과 같이 설정합니다:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 중요 런타임 참고 사항(현재 코드 동작)

현재 스크립트는 하드코딩된 플레이스홀더로 Groq 클라이언트를 초기화합니다:

```python
client = Groq(api_key="your_groq_api_key")
```

`GROQ_API_KEY`를 환경 변수로만 설정한 경우에는 `os.environ`에서 읽도록 스크립트를 수정하거나 플레이스홀더 값을 직접 바꿔야 합니다. 그렇지 않으면 인증 요청이 실패할 수 있습니다.

## ▶️ 사용법

챗봇을 실행하려면 다음을 실행합니다:

```bash
python voice_to_voice_chatbot.py
```

또는 Google Colab에서:

스크립트를 코드 셀에 복사해 실행합니다.

Gradio 인터페이스가 로컬에서 실행되며 챗봇과 상호작용할 수 있습니다.

### 챗봇과 상호작용하기

- **마이크 사용**: 마이크에 직접 말하세요. 챗봇이 음성을 전사하고 응답을 생성해 오디오로 재생합니다.
- **오디오 업로드**: 미리 녹음한 오디오 파일을 업로드하세요. 챗봇이 오디오를 전사하고 응답을 생성해 합성 오디오를 재생합니다.

## 🎬 사용 예시

### 예시 음성 플로우

1. 녹음: "Python을 빠르게 배우는 세 가지 팁은 무엇인가요?"
2. Whisper가 프롬프트 텍스트를 전사합니다.
3. Groq LLaMA 모델이 답변을 생성합니다.
4. gTTS가 MP3 응답을 만듭니다.
5. Gradio가 응답 텍스트와 오디오 플레이어를 표시합니다.

### 실행 예시

```bash
python voice_to_voice_chatbot.py
```

예상 결과: 브라우저에서 로컬 Gradio 앱이 열리며, 오디오 입력 1개와 두 개의 출력(텍스트 + 오디오)이 표시됩니다.

## 🧪 개발 노트

- 메인 파이프라인 함수: `chatbot_pipeline(audio_path)`
- Whisper 모델은 시작 시 `whisper.load_model("base")`로 로드됩니다.
- 임시 MP3 파일은 `NamedTemporaryFile(..., delete=False)`로 생성됩니다.
- 현재 오류 처리는 UI에 `(str(e), None)`을 반환합니다.
- `requirements.txt`에는 `whisper`와 `openai-whisper`가 모두 포함되어 있을 수 있어 환경에 따라 중복으로 동작할 수 있습니다.

## 🐞 문제 해결

### 일반적 문제

`ModuleNotFoundError`: 올바른 Whisper 패키지가 설치되었는지 확인하세요:

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: 환경 변수 또는 스크립트에서 키가 유효한지 확인하세요.

추가 점검 사항:

- 앱이 인증 오류로 실패하면 `voice_to_voice_chatbot.py`의 하드코딩된 `api_key="your_groq_api_key"`를 확인하세요.
- 마이크 캡처가 안 될 때는 오디오 파일 업로드로 STT → LLM → TTS 파이프라인이 동작하는지 먼저 확인하세요.
- 응답 오디오가 비어 있다면 gTTS와 Groq에 대한 아웃바운드 네트워크 접근이 가능한지 점검하세요.

### 빠른 진단 체크리스트

| 항목 | 검증 |
|---|---|
| API 키 연결 | `Groq(api_key=...)`가 플레이스홀더로 남아있지 않음 |
| Whisper 설치 | `openai-whisper`가 오류 없이 import됨 |
| 네트워크 경로 | Groq + gTTS로의 아웃바운드 접근 가능 |
| 오디오 소스 | 마이크 권한이 활성화되었거나 파일 업로드가 정상 동작 |

## 🗺️ 로드맵

- 기본 동작에서 Groq API 키를 환경 변수로 직접 읽도록 변경합니다.
- 파이프라인 헬퍼 함수 테스트 추가.
- 환경 변수 또는 CLI 인수를 통한 선택적 모델/설정 옵션을 추가합니다.
- 배포 옵션 추가(예: Docker, Hugging Face Spaces).

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 기여

기여를 환영합니다. 저장소를 포크한 뒤 새 브랜치를 만들고 변경 내용을 담은 풀 요청(Pull Request)을 제출하세요.

권장 기여 흐름:

1. 저장소를 포크하고 복제합니다.
2. 기능 브랜치를 만듭니다.
3. 변경 사항을 구현하고 검증합니다.
4. 변경 이유가 명확한 설명과 함께 풀 요청을 엽니다.

## 📄 라이선스

이 프로젝트는 현재 MIT 라이선스로 문서화되어 있습니다. 자세한 내용은 `LICENSE` 파일을 확인하세요.

가정: `LICENSE` 파일이 의도되어 있지만 현재 저장소 스냅샷에는 아직 존재하지 않을 수 있습니다.
