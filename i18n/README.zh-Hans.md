[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# 基于 Whisper、LLaMA 与 Groq API 的语音到语音聊天机器人

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

该项目是一个实时语音到语音聊天机器人：使用 OpenAI Whisper 做语音转文字，使用 Groq API 上的 LLaMA 8B 生成回复，再用 Google Text-to-Speech（gTTS）把文本转回语音。聊天界面基于 Gradio 构建，用户可以直接说话或上传音频文件与机器人交互。

## 概览

该应用在一个 Python 脚本中实现了完整的音频对话闭环：

1. 从麦克风或上传文件接收用户音频。
2. 使用 Whisper（`base` 模型）将语音转写为文本。
3. 通过 Groq（`llama3-8b-8192`）生成回复。
4. 使用 gTTS 将回复文本转换为 MP3 语音。
5. 在 Gradio Web UI 中同时返回回复文本与可播放音频。

### 对话流水线

| 阶段 | 组件 | 输出 |
|---|---|---|
| 🎙️ 输入 | Gradio Audio（麦克风/文件） | 音频路径 |
| 📝 转写 | Whisper `base` | 用户文本 |
| 🧠 推理 | Groq + `llama3-8b-8192` | 助手文本 |
| 🔊 语音合成 | gTTS | MP3 回复 |
| 🖥️ 交付 | Gradio UI | 文本 + 可播放音频 |

## 功能特性

- **语音转文字（Speech-to-Text）**：使用 OpenAI Whisper 模型将口语转换为文本。
- **AI 生成回复**：通过 Groq API 调用 LLaMA 8B，根据转写文本生成智能回复。
- **文字转语音（Text-to-Speech）**：使用 Google Text-to-Speech（gTTS）将生成的文本回复转回语音。
- **实时交互**：机器人支持实时工作，用户可通过麦克风或网页上传音频文件进行交互。
- **单文件简洁运行**：完整聊天流程集中在 `voice_to_voice_chatbot.py` 中，便于快速实验。

## 项目结构

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # 运行聊天机器人的主脚本
├── requirements.txt                        # Python 依赖列表
├── README.md                               # 项目文档（英文）
├── i18n/                                   # 本地化 README（在流水线中规划/生成）
└── .auto-readme-work/20260228_230442/      # README 自动化上下文/产物
```

来自 canonical README 的旧结构参考：

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # Main script to run the chatbot
├── requirements.txt           # List of Python dependencies
├── README.md                  # Project documentation
└── .gitignore                 # Git ignore file
```

## 前置要求

开始前，请确保满足以下条件：

- 本地机器或 Google Colab 上已安装 Python 3.7 或更高版本。
- 需要一个 Groq API Key。可在[这里](https://groq.com/)申请。
- [Google Colab](https://colab.research.google.com/) 或已安装必要库的本地 Python 环境。
- 需要联网用于：
  - 初次下载 Whisper 模型。
  - 调用 Groq API。
  - 生成 gTTS 音频。

### 要求速览

| 要求 | 用途说明 |
|---|---|
| Python `3.7+` | 运行聊天机器人脚本及其依赖 |
| Groq API Key | 认证访问 LLaMA 模型推理 |
| Colab 或本地环境 | 运行 Gradio + ML 相关库 |
| 网络连接 | Whisper 模型下载、Groq 请求、gTTS 合成 |

## 安装

按照以下步骤完成项目设置：

1. **克隆仓库：**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **安装依赖：**

安装所需的 Python 库：

```bash
pip install -r requirements.txt
```

或者，如果你使用 Google Colab，可通过以下命令安装：

```python
!pip install gradio groq-api openai-whisper gtts
```

## 配置

### 设置 Groq API Key

将你的 Groq API Key 添加到环境变量：

```bash
export GROQ_API_KEY='your_groq_api_key'
```

在 Google Colab 中，可通过以下方式设置：

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要运行说明（当前代码行为）

当前脚本使用硬编码占位符初始化 Groq 客户端：

```python
client = Groq(api_key="your_groq_api_key")
```

如果你只在环境变量中设置了 `GROQ_API_KEY`，需要把脚本改为从 `os.environ` 读取，或直接替换该占位符，否则 API 认证会失败。

## 使用方法

启动聊天机器人，运行主脚本：

```bash
python voice_to_voice_chatbot.py
```

或在 Google Colab 中：

将脚本复制到代码单元并执行。

Gradio 界面启动后，你就可以与聊天机器人交互。

### 与聊天机器人交互

- **使用麦克风**：直接对麦克风说话。机器人会转写语音、生成回复，并以音频形式播放。
- **上传音频**：上传预录音频文件。机器人会转写音频、生成回复，并将回复再转换为语音。

## 示例

### 示例语音流程

1. 录音：“What are three tips to learn Python quickly?”
2. Whisper 转写你的提示文本。
3. Groq LLaMA 模型生成答案。
4. gTTS 产出 MP3 回复。
5. Gradio 展示回复文本并提供音频播放。

### 示例运行命令

```bash
python voice_to_voice_chatbot.py
```

预期结果：浏览器会打开一个本地 Gradio 应用，包含 1 个音频输入和 2 个输出（文本 + 音频）。

## 开发说明

- 主流水线函数：`chatbot_pipeline(audio_path)`。
- Whisper 模型在启动时加载：`whisper.load_model("base")`。
- 临时 MP3 文件通过 `NamedTemporaryFile(..., delete=False)` 创建。
- 当前错误处理会向 UI 返回 `(str(e), None)`。
- `requirements.txt` 同时包含 `whisper` 与 `openai-whisper`；具体环境下这可能冗余。

## 故障排查

### 常见问题

`ModuleNotFoundError`：请确保安装了正确版本的 Whisper 模块：

```python
!pip install -U openai-whisper
```

`Groq API Key Error`：请再次确认你的 API Key 是否正确，并已正确配置到环境变量中。

更多排查建议：

- 如果应用启动后立即出现认证错误，请检查 `voice_to_voice_chatbot.py` 中是否仍是硬编码 `api_key="your_groq_api_key"`。
- 如果无法使用麦克风采集，可先上传音频文件，验证 STT -> LLM -> TTS 流程是否可用。
- 如果回复音频为空，请确认 gTTS 的外网访问是否可达。

### 快速诊断清单

| 检查项 | 验证方式 |
|---|---|
| API Key 接线 | `Groq(api_key=...)` 不是占位符 |
| Whisper 安装 | `openai-whisper` 可正常导入 |
| 网络路径 | 可向 Groq + gTTS 发起外网请求 |
| 音频来源 | 麦克风权限已开启或文件上传可用 |

## 路线图

- 默认从环境变量直接读取 Groq API Key。
- 为流水线辅助函数添加测试。
- 通过环境变量或 CLI 参数添加可选模型/配置。
- 在 `i18n/` 下补齐与语言导航链接对应的多语言 README 文件。
- 增加部署选项（例如 Docker 或 Hugging Face Spaces）。

## 贡献

欢迎贡献！请 fork 本仓库、创建新分支，并提交包含变更的 pull request。

建议的贡献流程：

1. Fork 并克隆仓库。
2. 创建功能分支。
3. 完成修改并测试。
4. 提交带有清晰说明的 pull request。

## 支持

当前仓库内容中未发现明确的捐赠/赞助链接。若维护者后续添加支持渠道，可在此处列出。

## 许可证

本项目采用 MIT 许可证。详情请参见 LICENSE 文件。

说明：当前仓库快照中可能暂未包含 `LICENSE` 文件，但默认应存在该文件。
