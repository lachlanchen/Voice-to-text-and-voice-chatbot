[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Whisper、LLaMA、Groq API を使った Voice-to-Voice チャットボット

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

このプロジェクトは、音声をテキストに書き起こす OpenAI の Whisper、Groq API 経由の LLaMA 3 8B による応答生成、Google Text-to-Speech（gTTS）によるテキスト再変換を組み合わせたリアルタイム音声対話チャットボットです。ユーザーは Gradio を介して、マイクで話すか音声ファイルをアップロードして操作できます。

## 概要

このアプリは、1 つの Python スクリプトで音声対話のループ全体を実装します。

1. マイクまたはアップロード済みファイルからユーザーの音声を受け取る。
2. Whisper（`base`）モデルで音声をテキスト化する。
3. Groq（`llama3-8b-8192`）で応答を生成する。
4. gTTS で応答テキストを MP3 音声に変換する。
5. 応答テキストと再生可能な音声を Gradio の Web UI 上で返す。

### 会話パイプライン

| ステージ | コンポーネント | 出力 |
|---|---|---|
| 🎙️ 入力 | Gradio Audio（マイク/ファイル） | 音声パス |
| 📝 転写 | Whisper `base` | ユーザーのテキスト |
| 🧠 推論 | Groq + `llama3-8b-8192` | アシスタントのテキスト |
| 🔊 合成 | gTTS | MP3 応答 |
| 🖥️ 配信 | Gradio UI | テキスト + 再生可能音声 |

## ⭐ 特徴

- **Speech-to-Text**: OpenAI の Whisper モデルを使って音声をテキストに変換します。
- **AI 生成レスポンス**: Groq API 経由で LLaMA 8B を使い、文字起こしされたテキストに基づいて賢い回答を生成します。
- **Text-to-Speech**: 応答テキストを Google Text-to-Speech（gTTS）で再度音声に変換します。
- **リアルタイム対話**: Web インターフェース上で、マイク入力または音声ファイルアップロードによる対話が可能です。
- **単一ファイルで完結する実行**: チャットボットの全パイプラインは `voice_to_voice_chatbot.py` に実装されています。
- **多言語ドキュメント**: `i18n/` には言語別にローカライズされた README へのリンクがあります。

## 📁 プロジェクト構成

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

## ✅ 前提条件

開始する前に、次の要件を満たしていることを確認してください。

- ローカルマシンまたは Google Colab に Python 3.7 以上がインストールされていること。
- Groq API キー。キーは [Groq](https://groq.com/) で取得できます。
- [Google Colab](https://colab.research.google.com/) または必要なライブラリが入ったローカル Python 環境。
- 以下のためのインターネットアクセス:
  - Whisper モデルの初回ダウンロード。
  - Groq API 呼び出し。
  - gTTS の音声生成。

### 要件サマリー

| 要件 | 必要な理由 |
|---|---|
| Python `3.7+` | チャットボットスクリプトと依存関係の実行環境 |
| Groq API キー | LLaMA モデル推論への認証付きアクセス |
| Colab またはローカル環境 | Gradio + ML ライブラリの実行環境 |
| インターネットアクセス | Whisper モデルのダウンロード、Groq リクエスト、gTTS 合成 |

## 🛠️ インストール

プロジェクトをセットアップする手順は次のとおりです。

1. **リポジトリをクローンする**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **依存関係をインストールする**

必要な Python ライブラリをインストールします。

```bash
pip install -r requirements.txt
```

Google Colab で代替する場合:

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ 設定

### Groq API キーの設定

Groq API キーをエクスポートします。

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Google Colab ではランタイム内で設定します。

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要な実行時メモ（現在のコード動作）

現在のスクリプトは、ハードコードされたプレースホルダーで Groq クライアントを初期化しています。

```python
client = Groq(api_key="your_groq_api_key")
```

環境変数 `GROQ_API_KEY` のみを設定した場合は、スクリプトを `os.environ` から読み取るように更新するか、プレースホルダーを直接置き換えてください。変更しないと認証呼び出しが失敗する可能性があります。

## ▶️ 利用方法

チャットボットを起動するには、次を実行します。

```bash
python voice_to_voice_chatbot.py
```

Google Colab の場合:

スクリプトをコードセルにコピーして実行します。

Gradio インターフェースがローカルで起動し、チャットボットと対話できます。

### チャットボットとの対話

- **マイクの使用**: マイクに直接話しかけてください。チャットボットが文字起こし、応答生成、音声再生を行います。
- **音声アップロード**: 事前録音した音声ファイルをアップロードします。チャットボットが文字起こしし、応答を生成して音声で再生します。

## 🎬 例

### 音声フローの例

1. 録音: "What are three tips to learn Python quickly?"
2. Whisper がプロンプトテキストを文字起こしします。
3. Groq LLaMA モデルが回答を生成します。
4. gTTS が MP3 応答を生成します。
5. Gradio が応答テキストと音声プレーヤーを表示します。

### 実行コマンド例

```bash
python voice_to_voice_chatbot.py
```

期待される結果: ブラウザでローカルの Gradio アプリが開き、1 つの音声入力と 2 つの出力（テキスト + 音声）が表示されます。

## 🧪 開発ノート

- メインのパイプライン関数: `chatbot_pipeline(audio_path)`。
- Whisper モデルは起動時に `whisper.load_model("base")` で読み込まれます。
- 一時 MP3 ファイルは `NamedTemporaryFile(..., delete=False)` で作成されます。
- 現在のエラーハンドリングは UI に `(str(e), None)` を返します。
- `requirements.txt` には `whisper` と `openai-whisper` の両方が含まれ、環境によっては冗長になる可能性があります。

## 🐞 トラブルシューティング

### 一般的な問題

`ModuleNotFoundError`: 正しい Whisper パッケージがインストールされていることを確認してください。

```python
!pip install -U openai-whisper
```

`Groq API キー エラー`: 環境またはスクリプト内でキーの有効性を確認してください。

追加のトラブルシューティング:

- アプリが認証エラーで失敗する場合、`voice_to_voice_chatbot.py` 内のハードコードされた `api_key="your_groq_api_key"` を確認してください。
- マイク取り込みが利用不可の場合は、まず音声ファイルをアップロードして STT → LLM → TTS パイプラインを検証してください。
- 応答音声が空の場合、gTTS と Groq への外部ネットワークアクセスを確認してください。

### クイック診断チェックリスト

| チェック | 検証 |
|---|---|
| API キーの接続 | `Groq(api_key=...)` がプレースホルダーのままになっていない |
| Whisper 導入 | `openai-whisper` が正常に import できる |
| ネットワーク経路 | Groq + gTTS への外部アクセスが可能 |
| 音声ソース | マイク権限が有効、またはファイルアップロードが機能 |

## 🗺️ ロードマップ

- デフォルトで環境変数から Groq API キーを直接読み込む。
- パイプライン補助関数向けのテストを追加する。
- 環境変数または CLI 引数での任意のモデル/設定追加。
- デプロイ方法を追加（例: Docker や Hugging Face Spaces）。

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 貢献

貢献は歓迎します。リポジトリをフォークし、新しいブランチを作成して、変更内容を含むプルリクエストを送信してください。

推奨の貢献フロー:

1. リポジトリをフォークしてクローンする。
2. 機能ブランチを作成する。
3. 変更を実装してテストする。
4. 明確な説明と理由を添えたプルリクエストを作成する。

## 📄 ライセンス

このプロジェクトは現在 MIT ライセンスでドキュメント化されています。詳細は LICENSE ファイルを参照してください。

前提: このリポジトリにはまだ `LICENSE` ファイルが存在しない可能性があります。
