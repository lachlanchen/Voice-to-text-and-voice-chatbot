[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Whisper, LLaMA, Groq API를 사용한 음성-음성 챗봇

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

이 프로젝트는 실시간 음성-음성 챗봇입니다. OpenAI Whisper로 음성을 텍스트로 전사하고, Groq API를 통한 LLaMA 8B로 답변을 생성한 뒤, Google Text-to-Speech(gTTS)로 텍스트를 다시 음성으로 변환합니다. 챗봇 인터페이스는 Gradio로 구현되어 있어, 사용자가 직접 말하거나 오디오 파일을 업로드해 상호작용할 수 있습니다.

## 개요

이 앱은 하나의 Python 스크립트에서 전체 오디오 대화 루프를 구현합니다:

1. 마이크 입력 또는 업로드 파일로 사용자 오디오를 받습니다.
2. Whisper(`base` 모델)로 음성을 텍스트로 전사합니다.
3. Groq(`llama3-8b-8192`)를 통해 답변을 생성합니다.
4. gTTS를 사용해 답변 텍스트를 MP3 오디오로 다시 변환합니다.
5. Gradio 웹 UI에서 답변 텍스트와 재생 가능한 오디오를 함께 반환합니다.

### 대화 파이프라인

| 단계 | 구성 요소 | 출력 |
|---|---|---|
| 🎙️ 입력 | Gradio Audio (mic/file) | Audio path |
| 📝 전사 | Whisper `base` | User text |
| 🧠 추론 | Groq + `llama3-8b-8192` | Assistant text |
| 🔊 합성 | gTTS | MP3 response |
| 🖥️ 전달 | Gradio UI | Text + playable audio |

## 기능

- **Speech-to-Text**: OpenAI Whisper 모델을 사용해 음성을 텍스트로 변환합니다.
- **AI-Generated Responses**: 전사된 텍스트를 바탕으로 Groq API의 LLaMA 8B가 지능형 답변을 생성합니다.
- **Text-to-Speech**: 생성된 텍스트 답변을 Google Text-to-Speech(gTTS)로 다시 음성으로 변환합니다.
- **Real-Time Interaction**: 웹 인터페이스에서 마이크 입력 또는 오디오 파일 업로드를 통해 실시간으로 상호작용할 수 있습니다.
- **Simple Single-File Runtime**: 전체 챗봇 파이프라인이 `voice_to_voice_chatbot.py` 하나에 구현되어 있어 실험이 쉽습니다.

## 프로젝트 구조

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # 챗봇 실행 메인 스크립트
├── requirements.txt                        # Python 의존성 목록
├── README.md                               # 프로젝트 문서 (영어)
├── i18n/                                   # 다국어 README (파이프라인에서 계획/생성)
└── .auto-readme-work/20260228_230442/      # README 자동화 컨텍스트/산출물
```

정식 README의 레거시 구조 참조:

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # 챗봇 실행 메인 스크립트
├── requirements.txt           # Python 의존성 목록
├── README.md                  # 프로젝트 문서
└── .gitignore                 # Git ignore 파일
```

## 사전 요구사항

시작하기 전에 다음 요구사항을 충족했는지 확인하세요:

- 로컬 머신 또는 Google Colab에 Python 3.7 이상이 설치되어 있어야 합니다.
- Groq API 키가 필요합니다. API 키 발급은 [여기](https://groq.com/)에서 할 수 있습니다.
- [Google Colab](https://colab.research.google.com/) 또는 필수 라이브러리가 설치된 로컬 Python 환경이 필요합니다.
- 다음 작업을 위한 인터넷 연결이 필요합니다:
  - 초기 Whisper 모델 다운로드
  - Groq API 호출
  - gTTS 오디오 생성

### 한눈에 보는 요구사항

| 요구사항 | 필요한 이유 |
|---|---|
| Python `3.7+` | 챗봇 스크립트 및 의존성 실행 런타임 |
| Groq API Key | LLaMA 모델 추론에 대한 인증된 접근 |
| Colab or Local Env | Gradio + ML 라이브러리 실행 환경 |
| Internet Access | Whisper 모델 다운로드, Groq 요청, gTTS 합성 |

## 설치

다음 단계에 따라 프로젝트를 설정하세요:

1. **저장소 클론:**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **의존성 설치:**

필요한 Python 라이브러리를 설치합니다:

```bash
pip install -r requirements.txt
```

또는 Google Colab을 사용하는 경우 다음과 같이 설치할 수 있습니다:

```python
!pip install gradio groq-api openai-whisper gtts
```

## 설정

### Groq API Key 설정

환경 변수에 Groq API 키를 추가하세요:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Google Colab에서는 다음과 같이 API 키를 설정할 수 있습니다:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 중요 런타임 참고 사항 (현재 코드 동작)

현재 스크립트는 하드코딩된 플레이스홀더로 Groq 클라이언트를 초기화합니다:

```python
client = Groq(api_key="your_groq_api_key")
```

환경 변수에 `GROQ_API_KEY`만 설정했다면, 스크립트가 `os.environ`을 읽도록 수정하거나 플레이스홀더를 직접 교체해야 합니다. 그렇지 않으면 API 인증이 실패합니다.

## 사용 방법

챗봇을 시작하려면 메인 스크립트를 실행하세요:

```bash
python voice_to_voice_chatbot.py
```

또는 Google Colab에서는:

스크립트를 코드 셀에 복사해 실행하세요.

Gradio 인터페이스가 실행되며 챗봇과 상호작용할 수 있습니다.

### 챗봇과 상호작용하기

- **마이크 사용**: 마이크에 직접 말하면 챗봇이 음성을 전사하고 답변을 생성한 뒤 오디오로 재생합니다.
- **오디오 업로드**: 사전 녹음된 오디오 파일을 업로드하면 챗봇이 오디오를 전사하고 답변을 생성한 뒤 다시 음성으로 변환합니다.

## 예시

### 음성 플로우 예시

1. 녹음: "What are three tips to learn Python quickly?"
2. Whisper가 프롬프트 텍스트를 전사합니다.
3. Groq LLaMA 모델이 답변을 생성합니다.
4. gTTS가 MP3 답변을 생성합니다.
5. Gradio가 답변 텍스트와 오디오 재생 UI를 표시합니다.

### 실행 명령 예시

```bash
python voice_to_voice_chatbot.py
```

예상 결과: 브라우저에서 로컬 Gradio 앱이 열리고, 오디오 입력 1개와 출력 2개(텍스트 + 오디오)가 표시됩니다.

## 개발 노트

- 메인 파이프라인 함수: `chatbot_pipeline(audio_path)`.
- Whisper 모델은 시작 시 로드됩니다: `whisper.load_model("base")`.
- 임시 MP3 파일은 `NamedTemporaryFile(..., delete=False)`로 생성됩니다.
- 현재 오류 처리는 UI에 `(str(e), None)`을 반환합니다.
- `requirements.txt`에는 `whisper`와 `openai-whisper`가 모두 포함되어 있으며, 환경에 따라 중복일 수 있습니다.

## 문제 해결

### 자주 발생하는 문제

`ModuleNotFoundError`: 아래 명령으로 올바른 버전의 Whisper 모듈을 설치했는지 확인하세요.

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: API 키를 다시 확인하고 환경 변수에 올바르게 설정되었는지 점검하세요.

추가 문제 해결:

- 앱이 인증 오류로 즉시 실패하면 `voice_to_voice_chatbot.py`의 하드코딩된 `api_key="your_groq_api_key"`를 확인하세요.
- 마이크 캡처를 사용할 수 없다면, 먼저 오디오 파일 업로드로 STT -> LLM -> TTS 파이프라인을 검증하세요.
- 응답 오디오가 비어 있다면 gTTS를 위한 외부 네트워크 연결이 가능한지 확인하세요.

### 빠른 진단 체크리스트

| 점검 항목 | 검증 내용 |
|---|---|
| API key wiring | `Groq(api_key=...)`가 플레이스홀더로 남아있지 않음 |
| Whisper install | `openai-whisper`가 정상 import됨 |
| Network path | Groq + gTTS로의 outbound 접근 가능 |
| Audio source | 마이크 권한 활성화 또는 파일 업로드 동작 |

## 로드맵

- 기본 동작으로 Groq API 키를 환경 변수에서 직접 읽도록 개선.
- 파이프라인 헬퍼 함수 테스트 추가.
- 환경 변수 또는 CLI 인자를 통한 선택적 모델/설정 옵션 추가.
- 언어 네비게이션 링크와 일치하도록 `i18n/` 아래 i18n README 파일 추가.
- 배포 옵션 추가(예: Docker 또는 Hugging Face Spaces).

## 기여

기여를 환영합니다. 이 저장소를 포크한 뒤 새 브랜치를 만들고, 변경 사항을 포함한 Pull Request를 제출해 주세요.

권장 기여 절차:

1. 저장소를 포크하고 클론합니다.
2. 기능 브랜치를 생성합니다.
3. 변경 사항을 작성하고 테스트합니다.
4. 변경 내용을 명확히 설명한 Pull Request를 엽니다.

## 지원

현재 저장소 내용에서는 명시적인 기부/후원 링크를 찾지 못했습니다. 유지관리자가 지원 채널을 추가하면 여기에 기재되어야 합니다.

## 라이선스

이 프로젝트는 MIT 라이선스를 따릅니다. 자세한 내용은 LICENSE 파일을 참조하세요.

가정: `LICENSE` 파일이 의도되어 있으나 현재 저장소 스냅샷에는 아직 없을 수 있습니다.
