[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# 使用 Whisper、LLaMA 和 Groq API 的语音到语音聊天机器人

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20Text-to-Speech-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)
![Platform](https://img.shields.io/badge/Runtime-Local%20%2F%20Colab-0EA5E9?logo=jupyter&logoColor=white)

本仓库提供一个紧凑的单文件语音聊天机器人。它采集语音，用 Whisper 转写文本，使用 Groq 托管的 LLaMA 进行推理，并通过 Google Text-to-Speech（gTTS）合成语音回复。面向终端用户的交互由 Gradio 负责，支持文本与音频两种输出。

> **目标：** 可在本地或 Colab 中运行的、可复现、实用的单脚本流水线。

## 🧭 快速快照

| 区域 | 状态 |
|---|---|
| 语言范围 | `README.md` 及 `i18n/` 下的多语言副本 |
| 真实来源 | 英文根 README 驱动本地化同步 |
| 建议运行模式 | 优先 `本地`，其次 `Colab` |

## 🔎 快速快照详情

| 关注点 | 状态 |
|---|---|
| 入口点 | `voice_to_voice_chatbot.py` |
| 界面 | 基于 Gradio 的网页 UI，支持文本 + 音频 |
| STT 模型 | Whisper（`base`） |
| LLM 后端 | Groq 托管的 `llama3-8b-8192` |
| TTS 引擎 | Google Text-to-Speech |
| 语言文档 | `i18n/` 下有 10+ 个翻译版本 |

## 概览

应用在 `voice_to_voice_chatbot.py` 中实现了端到端对话流水线：

1. 从麦克风输入或文件上传接收用户音频。
2. 使用 Whisper（`base`）模型将语音转写为文本。
3. 使用 Groq 和 `llama3-8b-8192` 生成回复。
4. 使用 gTTS 将生成文本转换为 MP3。
5. 在 Gradio 中渲染文本回复与播放控件。

### 对话流水线

| 阶段 | 组件 | 输出 |
|---|---|---|
| 🎙️ 输入 | `gr.Audio(type="filepath")` | 音频文件路径 |
| 📝 转录 | Whisper `base` 模型 | 转写文本 |
| 🧠 推理 | Groq 聊天补全 | 助手回复文本 |
| 🔊 合成 | `gTTS` | MP3 回复路径 |
| 🖥️ 交付 | Gradio `Interface` | 回复文本 + 音频播放 |

```mermaid
flowchart LR
    A[Audio Input] --> B[Whisper Transcription]
    B --> C[Groq LLaMA Reasoning]
    C --> D[gTTS Synthesis]
    D --> E[Gradio Output: Text + MP3]
```

## ⭐ 特性

- **一体化的 STT + LLM + TTS**：在 `voice_to_voice_chatbot.py` 中完成完整语音闭环。
- **麦克风与文件支持**：可选择实时发言或上传已录音频。
- **轻量化配置**：仅需少量 Python 包。
- **多语言文档**：`i18n/` 下维护本地化 README。
- **实用调试可见性**：函数级错误返回会在 UI 中直接展示，便于快速迭代。

## 📁 项目结构

```text
Voice-to-text-and-voice-chatbot/
    ├── requirements.txt              # Python 依赖
    ├── voice_to_voice_chatbot.py     # 主应用脚本
    ├── i18n/                        # 已翻译的 README 文件
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
└── .auto-readme-work/            # 用于 README 生成的元数据
    ├── 20260228_230442/
    ├── 20260301_064403/
    └── 20260301_065134/
        ├── language-nav-i18n.md
        ├── language-nav-root.md
        ├── pipeline-context.md
        └── translation-plan.txt
```

## 🌍 本地化与文档

该 README 项目以英文版为单一事实来源，并在 `i18n/` 下提供多语言版本。

- 使用本文件顶部附近的语言链接切换不同翻译版本。
- 现有翻译覆盖 10+ 种语言，需与英文结构保持同步。
- 建议先更新英文 README，再将翻译文件与关键结构和命令变更保持一致。

## ✅ 先决条件

- Python 3.7+ 运行环境。
- 有效的 Groq API Key。
- 需要网络访问以下载 Whisper 模型并调用 API。
- 可选：若使用实时音频，需要浏览器麦克风权限。
- 可选：GPU 可改善 Whisper 转写延迟与稳定性。

### 一览表

| 需求 | 作用 |
|---|---|
| Python `3.7+` | Gradio、Whisper 与相关依赖的运行环境 |
| Groq API key | 调用 LLM 推理所需 |
| `requirements.txt` | 安装所有必需 Python 包 |
| 浏览器麦克风权限 | 通过 Gradio 启用语音输入 |

## 🛠️ 安装

1. 克隆仓库：

```bash
git clone <repo-url>
cd Voice-to-text-and-voice-chatbot
```

2. 安装依赖：

```bash
pip install -r requirements.txt
```

在 Google Colab 中使用：

```python
!pip install -U gradio openai-whisper gtts groq
```

### 说明

- requirements 中当前同时声明了 `whisper` 与 `openai-whisper`。
- 若遇到包冲突，请优先保留与你环境匹配的版本，并在确认后移除冗余安装。

## 🧯 运行准备清单

| 步骤 | 检查 |
|---|---|
| API Key | `GROQ_API_KEY` 或受信任的本地备用配置已正确设置 |
| 音频设备 | 浏览器麦克风已开启以进行实时输入 |
| 运行路径 | 已在项目根目录执行，且依赖已安装 |
| 输出路径 | mp3 回复的临时目录可写 |

## ⚙️ 配置

### 环境变量（推荐）

```bash
export GROQ_API_KEY='your_groq_api_key'
```

在 Colab 运行时：

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要运行说明（当前行为）

`voice_to_voice_chatbot.py` 当前按如下方式初始化 Groq：

```python
client = Groq(
    api_key="your_groq_api_key",
)
```

若你仅设置了 `GROQ_API_KEY`，请在运行前将脚本改为从 `os.getenv` 读取，或从可信本地环境变量硬编码：

```python
client = Groq(api_key=os.getenv("GROQ_API_KEY", "your_groq_api_key"))
```

### 约定与假设

- 该仓库预期在本地 Python 环境或 Colab 中运行。
- 当前快照中未包含独立的服务入口点或部署配置。

## ▶️ 使用方式

使用以下命令启动应用：

```bash
python voice_to_voice_chatbot.py
```

Gradio 将启动一个本地界面，包含一个音频输入和两个输出：

- `Response Text`
- `Response Audio`

### 与聊天机器人交互

- **麦克风**：点击录音并说话；音频会被转写、生成回复并回放。
- **上传文件**：选择音频文件，用于转写和回复生成。

## 🎬 示例

### 示例流程

1. 提问：“What are three tips to learn Python quickly?”
2. Whisper 返回转写结果。
3. Groq 生成回答。
4. gTTS 合成输出。
5. UI 显示文本和音频回复。

### 预期输出

- 成功转写会显示在回复文本框中。
- 在 Gradio 音频播放器中会看到非空的语音回复文件。

## 🧪 开发说明

- 核心函数：`chatbot_pipeline(audio_path)`。
- Whisper 在模块导入时以 `whisper.load_model("base")` 只加载一次。
- 音频输出使用 `NamedTemporaryFile(..., delete=False)` 保持 mp3 持久化。
- 错误路径返回 `(str(e), None)`，确保故障时 UI 依然响应。
- `iface.launch()` 在模块导入时触发；若用于库式调用，建议使用 `if __name__ == "__main__":` 包裹启动逻辑。

## 🐞 故障排查

### 常见问题

- `ModuleNotFoundError`（Whisper）：

```bash
pip install -U openai-whisper
```

- Groq 身份验证失败：
  - 确认占位 API Key 已替换或从环境变量加载。
  - 确认该 Key 拥有足够权限与配额。

- 无输出音频：
  - 检查与 Groq 与 gTTS 的外部网络连通性。
  - 确认临时 MP3 路径在运行环境可写。

### 快速诊断清单

| 检查项 | 校验 |
|---|---|
| API Key 来源 | `Groq(api_key=...)` 是一个有效密钥 |
| STT 依赖 | `import whisper` 与 `openai-whisper` 导入成功 |
| 音频路径 | Gradio 收到有效音频文件路径 |
| 输出渲染 | UI 同时返回回复文本与音频 |

## 🗺️ 路线图

- 默认将硬编码的 Groq Key 替换为完全基于环境变量的配置。
- 增加基于环境变量的模型选择（Whisper 大小、Groq 模型 ID）。
- 增加核心函数的最小化测试。
- 增加 CLI 与部署预设（Docker/Hugging Face Spaces）。

## ♻️ 维护与同步策略

为了保持多语言 README 的一致性：

1. 先更新英文 `README.md`，用于结构或技术变更。
2. 将标题与关键内容同步镜像到 `i18n/` 的翻译文件。
3. 在本地化版本中保持 banner 与支持区块一致。

## 🤝 贡献

欢迎贡献。建议流程：

1. Fork 仓库。
2. 创建功能分支。
3. 实施你的修改。
4. 提交清晰说明和测试记录的 PR。

## 📄 许可证

本仓库标明了 MIT 许可证意图，但当前快照中未提供 `LICENSE` 文件。如果用于分发，请补充许可证文件。


## ❤️ Support

| Donate | PayPal | Stripe |
| --- | --- | --- |
| [![Donate](https://camo.githubusercontent.com/24a4914f0b42c6f435f9e101621f1e52535b02c225764b2f6cc99416926004b7/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f446f6e6174652d4c617a79696e674172742d3045413545393f7374796c653d666f722d7468652d6261646765266c6f676f3d6b6f2d6669266c6f676f436f6c6f723d7768697465)](https://chat.lazying.art/donate) | [![PayPal](https://camo.githubusercontent.com/d0f57e8b016517a4b06961b24d0ca87d62fdba16e18bbdb6aba28e978dc0ea21/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f50617950616c2d526f6e677a686f754368656e2d3030343537433f7374796c653d666f722d7468652d6261646765266c6f676f3d70617970616c266c6f676f436f6c6f723d7768697465)](https://paypal.me/RongzhouChen) | [![Stripe](https://camo.githubusercontent.com/1152dfe04b6943afe3a8d2953676749603fb9f95e24088c92c97a01a897b4942/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f5374726970652d446f6e6174652d3633354246463f7374796c653d666f722d7468652d6261646765266c6f676f3d737472697065266c6f676f436f6c6f723d7768697465)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |
