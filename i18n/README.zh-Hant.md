[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# 使用 Whisper、LLaMA 與 Groq API 的語音對語音聊天機器人

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

此專案是一個即時語音對語音聊天機器人，使用 OpenAI 的 Whisper 進行語音轉文字、透過 Groq API 呼叫 LLaMA 8B 產生回應，並使用 Google Text-to-Speech（gTTS）把文字再轉回語音。聊天介面以 Gradio 建置，讓使用者可以透過說話或上傳音訊檔與機器人互動。

## 概覽

此應用在單一 Python 腳本中實作完整的音訊對話迴圈：

1. 從麥克風或上傳檔案接收使用者音訊。
2. 使用 Whisper（`base` 模型）將語音轉寫為文字。
3. 透過 Groq（`llama3-8b-8192`）產生回應。
4. 使用 gTTS 將回應文字轉為 MP3 音訊。
5. 在 Gradio 網頁介面中同時回傳回應文字與可播放音訊。

### 對話流程

| 階段 | 元件 | 輸出 |
|---|---|---|
| 🎙️ 輸入 | Gradio Audio（麥克風/檔案） | 音訊路徑 |
| 📝 轉寫 | Whisper `base` | 使用者文字 |
| 🧠 推理 | Groq + `llama3-8b-8192` | 助理文字 |
| 🔊 合成 | gTTS | MP3 回應 |
| 🖥️ 傳遞 | Gradio UI | 文字 + 可播放音訊 |

## 功能特色

- **語音轉文字（Speech-to-Text）**：使用 OpenAI Whisper 模型將口語內容轉成文字。
- **AI 生成回應**：透過 Groq API 使用 LLaMA 8B，根據轉寫後文字生成智慧回覆。
- **文字轉語音（Text-to-Speech）**：使用 Google Text-to-Speech（gTTS）把生成文字回應轉回語音。
- **即時互動**：聊天機器人可即時運作，使用者可透過麥克風或網頁介面上傳音訊檔進行互動。
- **簡潔單檔執行**：整個聊天機器人流程都在 `voice_to_voice_chatbot.py` 中，便於快速實驗。

## 專案結構

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # 執行聊天機器人的主腳本
├── requirements.txt                        # Python 相依套件清單
├── README.md                               # 專案文件（英文）
├── i18n/                                   # 在地化 README（於流程中規劃/產生）
└── .auto-readme-work/20260228_230442/      # README 自動化流程的脈絡/產物
```

以下為來自 canonical README 的舊版結構參考：

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # 執行聊天機器人的主腳本
├── requirements.txt           # Python 相依套件清單
├── README.md                  # 專案文件
└── .gitignore                 # Git ignore 檔案
```

## 先決條件

開始前，請先確認你已滿足以下條件：

- 在本機或 Google Colab 安裝 Python 3.7 以上版本。
- 一組 Groq API key。你可以在[這裡](https://groq.com/)申請。
- 已準備好 [Google Colab](https://colab.research.google.com/) 或已安裝必要函式庫的本機 Python 環境。
- 可連網以便：
  - 初次下載 Whisper 模型。
  - 呼叫 Groq API。
  - 產生 gTTS 音訊。

### 需求速覽

| 需求 | 需要原因 |
|---|---|
| Python `3.7+` | 聊天機器人腳本與相依套件的執行環境 |
| Groq API Key | 驗證後存取 LLaMA 模型推論 |
| Colab 或本機環境 | 執行 Gradio + ML 函式庫 |
| 網際網路連線 | Whisper 模型下載、Groq 請求、gTTS 合成 |

## 安裝

請依照以下步驟設定專案：

1. **複製儲存庫：**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **安裝相依套件：**

安裝所需 Python 函式庫：

```bash
pip install -r requirements.txt
```

如果你使用 Google Colab，也可以透過以下方式安裝：

```python
!pip install gradio groq-api openai-whisper gtts
```

## 設定

### 設定 Groq API Key

將你的 Groq API key 加入環境變數：

```bash
export GROQ_API_KEY='your_groq_api_key'
```

在 Google Colab 中，你可以這樣設定 API key：

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要執行注意事項（目前程式碼行為）

目前腳本以硬編碼占位值初始化 Groq client：

```python
client = Groq(api_key="your_groq_api_key")
```

若你只在環境中設定 `GROQ_API_KEY`，請將腳本改為從 `os.environ` 讀取，或直接替換占位值，否則 API 驗證會失敗。

## 使用方式

要啟動聊天機器人，請執行主腳本：

```bash
python voice_to_voice_chatbot.py
```

或在 Google Colab 中：

將腳本貼到程式碼儲存格並執行。

Gradio 介面會啟動，讓你與聊天機器人互動。

### 與聊天機器人互動

- **使用麥克風**：直接對麥克風說話。聊天機器人會轉寫你的語音、生成回應並以音訊播放。
- **上傳音訊**：上傳預先錄製的音訊檔。聊天機器人會轉寫音訊、生成回應，並將回應再次轉為語音。

## 範例

### 語音流程範例

1. 錄音：「What are three tips to learn Python quickly?」
2. Whisper 轉寫你的提示文字。
3. Groq LLaMA 模型生成答案。
4. gTTS 產生 MP3 回應。
5. Gradio 顯示回應文字與音訊播放。

### 執行指令範例

```bash
python voice_to_voice_chatbot.py
```

預期結果：本機瀏覽器會開啟 Gradio 應用，含一個音訊輸入與兩個輸出（文字 + 音訊）。

## 開發備註

- 主要流程函式：`chatbot_pipeline(audio_path)`。
- Whisper 模型於啟動時載入：`whisper.load_model("base")`。
- 透過 `NamedTemporaryFile(..., delete=False)` 建立暫存 MP3 檔。
- 目前錯誤處理會回傳 `(str(e), None)` 到 UI。
- `requirements.txt` 同時包含 `whisper` 與 `openai-whisper`；依環境而定可能有重複。

## 疑難排解

### 常見問題

`ModuleNotFoundError`：請確認你已安裝正確版本的 Whisper 模組：

```python
!pip install -U openai-whisper
```

`Groq API Key Error`：請再次確認 API key 是否正確，且已正確設定於環境變數。

其他排查建議：

- 若應用一啟動就出現驗證錯誤，請檢查 `voice_to_voice_chatbot.py` 中是否仍為硬編碼 `api_key="your_groq_api_key"`。
- 若無法使用麥克風收音，先上傳音訊檔以驗證 STT -> LLM -> TTS 流程。
- 若回應音訊為空，請確認可對外連線以使用 gTTS。

### 快速診斷清單

| 檢查項目 | 驗證內容 |
|---|---|
| API key 串接 | `Groq(api_key=...)` 不應保留占位值 |
| Whisper 安裝 | `openai-whisper` 可正常匯入 |
| 網路路徑 | 可對外連線至 Groq + gTTS |
| 音訊來源 | 麥克風權限已啟用或上傳檔案可用 |

## 路線圖

- 預設直接從環境變數讀取 Groq API key。
- 為流程輔助函式新增測試。
- 透過環境變數或 CLI 參數加入可選模型/設定。
- 在 `i18n/` 下新增與語言導覽連結一致的 i18n README 檔案。
- 新增部署選項（例如 Docker 或 Hugging Face Spaces）。

## 貢獻

歡迎貢獻！請 fork 此儲存庫、建立新分支，並透過 pull request 提交你的修改。

建議的貢獻流程：

1. Fork 並 clone 此儲存庫。
2. 建立功能分支。
3. 完成修改並測試。
4. 發送包含清楚說明的 pull request。

## 支援

目前儲存庫內容中未找到明確的捐助/贊助連結。若維護者新增支援管道，應列在此處。

## 授權

本專案採用 MIT License。詳情請參閱 LICENSE 檔案。

假設：此儲存庫快照預期應有 `LICENSE` 檔案，但目前可能尚未提供。
