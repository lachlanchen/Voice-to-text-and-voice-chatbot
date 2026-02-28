[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# 使用 Whisper、LLaMA 與 Groq API 的語音到語音聊天機器人

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20Text-to-Speech-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)
![Platform](https://img.shields.io/badge/Runtime-Local%20%2F%20Colab-0EA5E9?logo=jupyter&logoColor=white)

本專案提供一個精簡的單檔語音聊天機器人。它會擷取語音，使用 Whisper 轉錄文字，再將文字送給 Groq 托管的 LLaMA 進行推理，並透過 Google Text-to-Speech（gTTS）合成語音回應。使用者端互動由 Gradio 負責，並同時提供文字與音訊輸出。

> **目標：** 可在本機或 Colab 上，直接以單一主腳本執行的可重現且實用流程。

## 🧭 快速概覽

| 項目 | 狀態 |
|---|---|
| 語言範圍 | `README.md` 以及 `i18n/` 下的多語系複本 |
| 主要來源 | 英文根 README 驅動在地化同步 |
| 建議執行模式 | 優先 `本機`，次選 `Colab` |

## 🔎 快速概覽細節

| 關注點 | 狀態 |
|---|---|
| 進入點 | `voice_to_voice_chatbot.py` |
| 介面 | 以 Gradio 為基礎的網頁 UI，提供文字 + 音訊 |
| STT 模型 | Whisper（`base`） |
| LLM 後端 | Groq 托管的 `llama3-8b-8192` |
| TTS 引擎 | Google Text-to-Speech |
| 語言文件 | `i18n/` 內有 10+ 份翻譯 README |

## 概述

本應用在 `voice_to_voice_chatbot.py` 實作完整的端對端對話流程：

1. 透過麥克風輸入或上傳檔案接收使用者音訊。
2. 使用 Whisper（`base`）模型將語音轉為文字。
3. 使用 Groq 與 `llama3-8b-8192` 產生回應。
4. 用 gTTS 將產生文字轉換為 MP3。
5. 在 Gradio 中顯示文字回應並提供播放控制。

### 對話流程

| 階段 | 元件 | 輸出 |
|---|---|---|
| 🎙️ 輸入 | `gr.Audio(type="filepath")` | 音訊檔案路徑 |
| 📝 轉錄 | Whisper `base` 模型 | 轉錄文字 |
| 🧠 推理 | Groq chat completion | 助理回應文字 |
| 🔊 合成 | `gTTS` | MP3 回應路徑 |
| 🖥️ 呈現 | Gradio `Interface` | 回應文字 + 音訊播放 |

```mermaid
flowchart LR
    A[Audio Input] --> B[Whisper Transcription]
    B --> C[Groq LLaMA Reasoning]
    C --> D[gTTS Synthesis]
    D --> E[Gradio Output: Text + MP3]
```

## ⭐ 特點

- **STT + LLM + TTS 全部在一個腳本**：在 `voice_to_voice_chatbot.py` 中完成完整語音回路。
- **支援麥克風與檔案**：可選擇即時說話或上傳錄音檔。
- **輕量化設定**：只需少量 Python 套件。
- **多語言文件**：在 `i18n/` 內維護各語言 README。
- **實用除錯可見性**：函式層級錯誤回傳會直接在 UI 顯示，便於快速迭代。

## 📁 專案結構

```text
Voice-to-text-and-voice-chatbot/
    ├── requirements.txt              # Python 依賴套件
    ├── voice_to_voice_chatbot.py     # 主應用程式
    ├── i18n/                        # 翻譯過的 README 檔案
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
└── .auto-readme-work/            # 自動化 README 生成時產生的中介資料
    ├── 20260228_230442/
    ├── 20260301_064403/
    └── 20260301_065134/
        ├── language-nav-i18n.md
        ├── language-nav-root.md
        ├── pipeline-context.md
        └── translation-plan.txt
```

## 🌍 在地化與文件

本 README 專案以英文版為主要來源，並在 `i18n/` 提供各語言版本。

- 使用本檔上方的語言連結，可在不同翻譯版本間切換。
- 現有翻譯目前覆蓋 10+ 種語言，並應與英文結構保持同步。
- 建議先更新英文 README，再依照重要結構與指令變更對齊各語言檔。

## ✅ 前置條件

- Python 3.7+ 執行環境。
- 有效的 Groq API 金鑰。
- 需可存取網路以下載 Whisper 模型並呼叫 API。
- 選用：若使用即時音訊，瀏覽器需允許麥克風權限。
- 選用：GPU 可改善 Whisper 轉錄延遲與穩定度。

### 需求一覽

| 需求 | 為何需要 |
|---|---|
| Python `3.7+` | Gradio、Whisper 與相關套件的執行環境 |
| Groq API 金鑰 | 呼叫 LLM 推理所需 |
| `requirements.txt` | 安裝所有必要的 Python 套件 |
| 瀏覽器麥克風權限 | 透過 Gradio 啟用語音輸入 |

## 🛠️ 安裝

1. 複製專案：

```bash
git clone <repo-url>
cd Voice-to-text-and-voice-chatbot
```

2. 安裝依賴：

```bash
pip install -r requirements.txt
```

在 Google Colab 中請改用：

```python
!pip install -U gradio openai-whisper gtts groq
```

### 說明

- 專案目前在 requirements 中同時宣告了 `whisper` 與 `openai-whisper`。
- 若遇到套件衝突，請優先保留與你的環境相容的版本，並在確認後移除冗餘安裝。

## 🧯 可執行準備檢查清單

| 步驟 | 檢查 |
|---|---|
| API 金鑰 | `GROQ_API_KEY` 或受信任的本機備援設定已正確配置 |
| 音訊設備 | 已為即時輸入啟用瀏覽器麥克風 |
| 執行路徑 | 已在專案根目錄執行且套件已安裝 |
| 輸出路徑 | MP3 回應的暫存目錄可寫入 |

## ⚙️ 設定

### 環境變數（建議）

```bash
export GROQ_API_KEY='your_groq_api_key'
```

在 Colab 執行階段：

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要執行時註記（目前行為）

`voice_to_voice_chatbot.py` 目前以以下方式初始化 Groq：

```python
client = Groq(
    api_key="your_groq_api_key",
)
```

若你只設定了 `GROQ_API_KEY`，請在執行前將腳本改為用 `os.getenv` 讀取，或在可信本機環境變數中硬編碼：

```python
client = Groq(api_key=os.getenv("GROQ_API_KEY", "your_groq_api_key"))
```

### 假設條件

- 本專案預期於本機 Python 環境或 Colab 中執行。
- 此快照中未提供獨立的伺服器進入點或部署設定。

## ▶️ 使用方式

使用下列命令啟動應用：

```bash
python voice_to_voice_chatbot.py
```

Gradio 將啟動一個本地介面，包含一個音訊輸入與兩個輸出：

- `Response Text`
- `Response Audio`

### 與聊天機器人互動

- **麥克風**：點選錄音並說話；音訊會先轉錄、再回覆，最後播放結果。
- **上傳檔案**：選擇一個音訊檔，用於轉錄與回應生成。

## 🎬 範例

### 流程範例

1. 提問：「你如何快速學會 Python？」
2. Whisper 回傳轉錄結果。
3. Groq 產生答案。
4. gTTS 合成輸出。
5. UI 顯示文字回覆與音訊。

### 預期輸出

- 成功轉錄後會顯示於回應文字框。
- Gradio 音訊播放器中會出現非空的語音回應檔。

## 🧪 開發備註

- 核心函式：`chatbot_pipeline(audio_path)`。
- `Whisper` 在模組載入時以 `whisper.load_model("base")` 僅載入一次。
- 音訊輸出使用 `NamedTemporaryFile(..., delete=False)` 以保留 mp3。
- 錯誤流程會回傳 `(str(e), None)`，讓 UI 在失敗時維持回應能力。
- `iface.launch()` 會在模組載入時被呼叫；若要作為套件式用法，建議使用 `if __name__ == "__main__":` 包住啟動程式碼。

## 🐞 疑難排解

### 常見問題

- `ModuleNotFoundError`（Whisper）：

```bash
pip install -U openai-whisper
```

- Groq 認證失敗：
  - 確認預留 API Key 已更換，或已從環境變數載入。
  - 確認該金鑰擁有足夠權限與配額。

- 無音訊輸出：
  - 檢查與 Groq、gTTS 的外部網路連線。
  - 確認暫存 MP3 路徑在執行環境可寫入。

### 快速診斷清單

| 檢查項目 | 驗證 |
|---|---|
| API 金鑰來源 | `Groq(api_key=...)` 為有效金鑰 |
| STT 依賴 | `import whisper` 與 `openai-whisper` 匯入成功 |
| 音訊路徑 | Gradio 收到有效音訊檔案路徑 |
| 輸出渲染 | UI 會同時回傳回應文字與音訊 |

## 🗺️ 路線圖

- 由預設的硬編碼 Groq Key 改為完全以環境變數驅動。
- 新增環境變數控制的模型選擇（`whisper` 大小、Groq 模型 ID）。
- 為輔助函式新增最小測試。
- 新增 CLI 與部署預設值（Docker/Hugging Face Spaces）。

## ♻️ 維護與同步策略

為維持多語言 README 品質一致：

1. 若有結構或技術變更，請先更新英文版 `README.md`。
2. 將標題與關鍵內容同步到 `i18n/` 各語言檔。
3. 在在地化版本間保持 banner 與 support 區塊一致。

## 🤝 貢獻

歡迎任何形式的貢獻。建議流程如下：

1. Fork 該儲存庫。
2. 建立功能分支。
3. 實作你的變更。
4. 提出清楚說明修改依據與測試備註的 Pull Request。

## 📄 授權

本專案採用 MIT 授權意圖，但此快照中尚未提供 `LICENSE` 檔案。若預期公開發布，請補上授權檔案。


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
