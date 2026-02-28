[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# 基于 Whisper、LLaMA 与 Groq API 的语音到语音聊天机器人

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

本项目是一个实时语音到语音聊天机器人，使用 OpenAI 的 Whisper 进行语音转文本，使用 Groq API 上的 LLaMA 3 8B 生成回答，并用 Google Text-to-Speech（gTTS）将文本转回语音。界面基于 Gradio 构建，用户可通过语音输入或上传音频文件进行交互。

## 概览

应用程序在一个 Python 脚本中实现了完整的音频对话闭环：

1. 从麦克风或上传文件接收用户音频。
2. 使用 Whisper（`base`）模型将语音转录为文本。
3. 通过 Groq（`llama3-8b-8192`）生成回答。
4. 使用 gTTS 将回复文本转换为 MP3 音频。
5. 在 Gradio Web UI 中返回文本回答和可播放音频。

### 对话流水线

| 阶段 | 组件 | 输出 |
|---|---|---|
| 🎙️ 输入 | Gradio Audio（麦克风/文件） | 音频路径 |
| 📝 转写 | Whisper `base` | 用户文本 |
| 🧠 推理 | Groq + `llama3-8b-8192` | 助手文本 |
| 🔊 合成 | gTTS | MP3 回复 |
| 🖥️ 投递 | Gradio UI | 文本 + 可播放音频 |

## ⭐ 特性

- **语音转文本**：使用 OpenAI 的 Whisper 模型将口头语言转成文本。
- **AI 生成回复**：通过 Groq API 使用 LLaMA 8B，基于转录文本生成智能回复。
- **文字转语音**：使用 Google Text-to-Speech（gTTS）将回复文本转回语音。
- **实时交互**：通过麦克风或网页上传音频文件进行交互。
- **单文件轻量运行**：完整聊天流程在 `voice_to_voice_chatbot.py` 中实现。
- **多语言文档**：主 README 在 `i18n/` 中提供了语言版本链接。

## 📁 项目结构

```text
Voice-to-text-and-voice-chatbot/
├── README.md                     # 主项目文档（英文）
├── requirements.txt              # Python 依赖
├── voice_to_voice_chatbot.py     # 运行聊天机器人的主脚本
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

## ✅ 先决条件

开始前，请确保满足以下要求：

- 本地机器或 Google Colab 上已安装 Python 3.7 或更高版本。
- 一个 Groq API 密钥。你可以在 [Groq](https://groq.com/) 注册获取。
- [Google Colab](https://colab.research.google.com/) 或具有所需库的本地 Python 环境。
- 需要联网以便：
  - 首次下载 Whisper 模型。
  - 调用 Groq API。
  - 生成 gTTS 音频。

### 要点一览

| 需求 | 用途 |
|---|---|
| Python `3.7+` | 运行聊天机器人脚本及其依赖 |
| Groq API Key | 认证访问 LLaMA 模型推理 |
| Colab 或本地环境 | 运行 Gradio 与机器学习库 |
| 网络访问 | Whisper 模型下载、Groq 请求、gTTS 合成 |

## 🛠️ 安装

按以下步骤设置项目：

1. **克隆仓库**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **安装依赖**

安装所需的 Python 库：

```bash
pip install -r requirements.txt
```

如果你在 Google Colab 中使用，也可执行：

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ 配置

### 设置 Groq API Key

导出你的 Groq API Key：

```bash
export GROQ_API_KEY='your_groq_api_key'
```

在 Google Colab 中，可在运行时设置：

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要运行说明（当前代码行为）

当前脚本使用硬编码占位符初始化 Groq 客户端：

```python
client = Groq(api_key="your_groq_api_key")
```

如果你仅在环境变量中设置了 `GROQ_API_KEY`，请更新脚本改为读取 `os.environ`（或直接替换该占位符），否则认证调用可能失败。

## ▶️ 使用方法

启动聊天机器人，运行：

```bash
python voice_to_voice_chatbot.py
```

或在 Google Colab 中：

将脚本复制到代码单元并执行。

Gradio 界面将本地启动，届时你可以与聊天机器人互动。

### 与聊天机器人互动

- **使用麦克风**：直接对麦克风说话。聊天机器人会转录你的语音、生成回复并以音频回放。
- **上传音频**：上传预录音频文件。聊天机器人会转录录音、生成回复，并播放合成音频。

## 🎬 示例

### 示例语音流程

1. 录音输入："What are three tips to learn Python quickly?"
2. Whisper 将你的提示词转写为文本。
3. Groq LLaMA 模型生成答案。
4. gTTS 生成 MP3 回复。
5. Gradio 显示回复文本与音频播放器。

### 示例运行命令

```bash
python voice_to_voice_chatbot.py
```

预期结果：浏览器会打开一个本地 Gradio 应用，包含一个音频输入和两个输出（文本 + 音频）。

## 🧪 开发说明

- 主流水线函数：`chatbot_pipeline(audio_path)`。
- 启动时使用 `whisper.load_model("base")` 加载 Whisper 模型。
- 临时 MP3 文件使用 `NamedTemporaryFile(..., delete=False)` 创建。
- 错误处理当前会向 UI 返回 `(str(e), None)`。
- `requirements.txt` 同时包含 `whisper` 和 `openai-whisper`，可能在某些环境下是冗余的。

## 🐞 故障排查

### 常见问题

`ModuleNotFoundError`：确保安装了正确的 Whisper 软件包：

```python
!pip install -U openai-whisper
```

`Groq API Key Error`：确认你的环境或脚本中的密钥可用且有效。

附加排查：

- 如果应用出现认证错误，请检查 `voice_to_voice_chatbot.py` 中是否仍有硬编码的 `api_key="your_groq_api_key"`。
- 如果麦克风采集不可用，请先上传音频文件以验证 STT → LLM → TTS 流程。
- 如果回复音频为空，请确认 gTTS 和 Groq 的外网访问权限。

### 快速诊断清单

| 检查项 | 验证方式 |
|---|---|
| API Key 配置 | `Groq(api_key=...)` 未保留占位符 |
| Whisper 安装 | `openai-whisper` 可正常导入 |
| 网络通路 | Groq + gTTS 的出站访问可用 |
| 音频来源 | 麦克风权限已启用或文件上传正常 |

## 🗺️ 路线图

- 默认从环境变量直接读取 Groq API Key。
- 为流水线辅助函数添加测试。
- 通过环境变量或 CLI 参数新增可选模型与配置设置。
- 增加部署选项（例如 Docker 或 Hugging Face Spaces）。

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 贡献

欢迎贡献。请 fork 本仓库、创建新分支，并提交你的改动对应的 Pull Request。

建议的贡献流程：

1. Fork 并克隆仓库。
2. 创建功能分支。
3. 实施并测试你的更改。
4. 提交一份说明清晰的 Pull Request。

## 📄 许可证

该项目当前以 MIT 许可进行说明。详情见 LICENSE 文件。

假设：仓库快照中可能尚未包含 `LICENSE` 文件。
