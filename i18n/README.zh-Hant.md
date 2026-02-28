[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# 使用 Whisper、LLaMA 與 Groq API 的語音對語音聊天機器人

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

本專案是一個即時語音對語音的聊天機器人，使用 OpenAI 的 Whisper 進行語音轉文字，透過 Groq API 呼叫 LLaMA 3 8B 產生回應，並使用 Google Text-to-Speech（gTTS）將文字轉回語音。介面採用 Gradio 製作，使用者可透過口述或上傳音訊檔與機器人互動。

## 概覽

本應用程式在單一 Python 腳本中實作完整的音訊對話迴圈：

1. 接收來自麥克風或上傳檔案的使用者音訊。
2. 使用 Whisper（`base`）模型將語音轉錄為文字。
3. 透過 Groq（`llama3-8b-8192`）生成回應。
4. 使用 gTTS 將回應文字轉成 MP3 音訊。
5. 於 Gradio 網頁介面回傳回應文字與可播放音訊。

### 對話流程

| 階段 | 元件 | 輸出 |
|---|---|---|
| 🎙️ 輸入 | Gradio Audio（麥克風/檔案） | 音訊路徑 |
| 📝 轉錄 | Whisper `base` | 使用者文字 |
| 🧠 推理 | Groq + `llama3-8b-8192` | 助理文字 |
| 🔊 合成 | gTTS | MP3 回應 |
| 🖥️ 傳遞 | Gradio UI | 文字 + 可播放音訊 |

## ⭐ 功能

- **語音轉文字**：使用 OpenAI 的 Whisper 模型將口語轉成文字。
- **AI 生成回應**：透過 Groq API 使用 LLaMA 8B，依據轉錄文字生成智慧回覆。
- **文字轉語音**：使用 Google Text-to-Speech（gTTS）將回應文字再轉成語音。
- **即時互動**：可透過麥克風或上傳音訊檔，在網頁介面中互動。
- **簡潔單一檔案執行**：完整聊天機器人流程皆在 `voice_to_voice_chatbot.py` 實作。
- **多語言文件**：主 README 透過 `i18n/` 連結到語系版本。

## 📁 專案結構

```text
Voice-to-text-and-voice-chatbot/
├── README.md                     # 主專案文件（英文）
├── requirements.txt              # Python 相依套件
├── voice_to_voice_chatbot.py     # 執行聊天機器人的主腳本
├── i18n/                        # 本地化 README
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

## ✅ 先決條件

開始前，請確認你已符合以下條件：

- 本機或 Google Colab 上已安裝 Python 3.7 以上版本。
- Groq API 金鑰。你可以在 [Groq](https://groq.com/) 申請。
- 已準備好 [Google Colab](https://colab.research.google.com/) 或本機 Python 環境，並安裝必要函式庫。
- 具備網路連線以用於：
  - 初次下載 Whisper 模型。
  - 呼叫 Groq API。
  - 進行 gTTS 音訊合成。

### 需求一覽

| 需求 | 為何需要 |
|---|---|
| Python `3.7+` | 聊天機器人腳本與相依套件的執行環境 |
| Groq API Key | 取得 LLaMA 模型推論的驗證存取 |
| Colab 或本機環境 | 執行 Gradio 與機器學習函式庫 |
| 網路連線 | Whisper 模型下載、Groq 請求與 gTTS 合成 |

## 🛠️ 安裝

請依以下步驟設定專案：

1. **複製儲存庫**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **安裝相依套件**

安裝所需的 Python 函式庫：

```bash
pip install -r requirements.txt
```

或在 Google Colab 中執行：

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ 設定

### 設定 Groq API Key

將你的 Groq API key 匯出：

```bash
export GROQ_API_KEY='your_groq_api_key'
```

在 Google Colab 中，可在執行階段這樣設定：

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要執行說明（目前程式碼行為）

目前腳本使用硬編碼預留值初始化 Groq 用戶端：

```python
client = Groq(api_key="your_groq_api_key")
```

如果你僅設定 `GROQ_API_KEY` 環境變數，請更新腳本改為從 `os.environ` 讀取（或直接替換預留值），否則驗證請求可能失敗。

## ▶️ 使用方式

要啟動聊天機器人，請執行：

```bash
python voice_to_voice_chatbot.py
```

或在 Google Colab 中：

將腳本貼到程式碼儲存格並執行。

Gradio 介面會在本機啟動，讓你與聊天機器人互動。

### 與聊天機器人互動

- **使用麥克風**：直接對著麥克風說話。聊天機器人會轉錄你的語音、生成回應，並以音訊播放。
- **上傳音訊**：上傳預先錄製的音訊檔。聊天機器人會轉錄錄音、生成回應，並播放合成音訊。

## 🎬 範例

### 語音流程範例

1. 錄音：「What are three tips to learn Python quickly?」
2. Whisper 轉錄你的提示詞文字。
3. Groq LLaMA 模型生成答案。
4. gTTS 產生 MP3 回應。
5. Gradio 顯示回應文字並提供音訊播放器。

### 執行指令範例

```bash
python voice_to_voice_chatbot.py
```

預期結果：在瀏覽器中會開啟本機 Gradio 應用程式，包含一個音訊輸入與兩個輸出（文字 + 音訊）。

## 🧪 開發備註

- 主要流程函式：`chatbot_pipeline(audio_path)`。
- 啟動時載入 Whisper 模型：`whisper.load_model("base")`。
- 臨時 MP3 檔案使用 `NamedTemporaryFile(..., delete=False)` 建立。
- 目前的錯誤處理會回傳 `(str(e), None)` 給 UI。
- `requirements.txt` 同時包含 `whisper` 與 `openai-whisper`；視執行環境而定可能有重複。

## 🐞 疑難排解

### 常見問題

`ModuleNotFoundError`：請確認已安裝正確的 Whisper 套件：

```python
!pip install -U openai-whisper
```

`Groq API Key Error`：請確認你的金鑰在環境或腳本中可用且有效。

其他排查建議：

- 若應用程式因驗證失敗無法啟動，請確認 `voice_to_voice_chatbot.py` 中的 `api_key="your_groq_api_key"` 是否仍為硬編碼。
- 如果麥克風擷取無法使用，先上傳一個音訊檔來驗證 STT → LLM → TTS 流程。
- 若回應音訊為空，請確認 gTTS 與 Groq 的外部網路連線正常。

### 快速診斷檢查

| 檢查項目 | 驗證方式 |
|---|---|
| API key 串接 | `Groq(api_key=...)` 未留空占位符 |
| Whisper 安裝 | `openai-whisper` 匯入無誤 |
| 網路連線 | Groq 與 gTTS 可正常對外存取 |
| 音訊來源 | 麥克風權限已開啟或音訊上傳可用 |

## 🗺️ 路線圖

- 預設從環境變數直接讀取 Groq API key。
- 為流程輔助函式加入測試。
- 透過環境變數或 CLI 參數加入可選模型/設定。
- 新增部署選項（例如 Docker 或 Hugging Face Spaces）。

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 貢獻

歡迎提出貢獻。請先 fork 本儲存庫、新建分支，並提交含說明與理由的 pull request。

建議的貢獻流程：

1. Fork 並 clone 此儲存庫。
2. 建立功能分支。
3. 實作並測試你的修改。
4. 提交一則具體說明的 pull request。

## 📄 授權

本專案目前以 MIT 授權說明。詳情請參考 LICENSE 檔案。

假設：此儲存庫快照本應存在 `LICENSE` 檔案，但目前可能尚未提供。
