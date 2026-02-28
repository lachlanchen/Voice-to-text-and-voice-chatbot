[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Whisper、LLaMA、Groq API を使った Voice-to-Voice チャットボット

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

このプロジェクトはリアルタイムの音声対音声チャットボットです。OpenAI の Whisper で音声をテキストに書き起こし、Groq API 経由の LLaMA 8B で応答を生成し、Google Text-to-Speech（gTTS）でテキストを再び音声へ変換します。チャットボットのインターフェースは Gradio で構築されており、ユーザーは話しかけるか音声ファイルをアップロードして操作できます。

## 概要

このアプリは、1 つの Python スクリプトで音声会話のループ全体を実装しています。

1. マイク入力またはアップロードされたファイルからユーザー音声を受け取る。
2. Whisper（`base` モデル）で音声をテキスト化する。
3. Groq（`llama3-8b-8192`）経由で応答を生成する。
4. gTTS を使って応答テキストを MP3 音声に戻す。
5. Gradio の Web UI 上で応答テキストと再生可能な音声を返す。

### 会話パイプライン

| ステージ | コンポーネント | 出力 |
|---|---|---|
| 🎙️ 入力 | Gradio Audio（マイク/ファイル） | 音声パス |
| 📝 書き起こし | Whisper `base` | ユーザーテキスト |
| 🧠 推論 | Groq + `llama3-8b-8192` | アシスタントテキスト |
| 🔊 音声合成 | gTTS | MP3 応答 |
| 🖥️ 提供 | Gradio UI | テキスト + 再生可能音声 |

## 機能

- **Speech-to-Text**: OpenAI の Whisper モデルを使って話し言葉をテキストに変換します。
- **AI-Generated Responses**: Groq API 経由の LLaMA 8B を使い、書き起こされたテキストに基づいて応答を生成します。
- **Text-to-Speech**: 生成したテキスト応答を Google Text-to-Speech（gTTS）で音声に変換します。
- **Real-Time Interaction**: チャットボットはリアルタイム動作し、Web インターフェース経由でマイク入力または音声ファイルのアップロードに対応します。
- **Simple Single-File Runtime**: チャットボットのパイプライン全体を `voice_to_voice_chatbot.py` にまとめており、試行しやすい構成です。

## プロジェクト構成

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # Main script to run the chatbot
├── requirements.txt                        # List of Python dependencies
├── i18n/                                   # Localized READMEs (planned/generated in pipeline)
└── .auto-readme-work/20260228_230442/      # README automation context/artifacts
```

正本 README にある旧構成の参照:

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # Main script to run the chatbot
├── requirements.txt           # List of Python dependencies
├── README.md                  # Project documentation
└── .gitignore                 # Git ignore file
```

## 前提条件

開始前に、次の要件を満たしていることを確認してください。

- ローカルマシンまたは Google Colab に Python 3.7 以上がインストールされていること。
- Groq API キーを取得していること。API キーの登録は[こちら](https://groq.com/)。
- [Google Colab](https://colab.research.google.com/) または必要なライブラリを導入したローカルの Python 環境。
- 以下のためのインターネット接続:
  - Whisper モデルの初回ダウンロード。
  - Groq API 呼び出し。
  - gTTS による音声生成。

### 要件一覧

| 要件 | 必要な理由 |
|---|---|
| Python `3.7+` | チャットボットスクリプトと依存関係の実行環境 |
| Groq API Key | LLaMA モデル推論への認証付きアクセス |
| Colab or Local Env | Gradio + ML ライブラリの実行環境 |
| Internet Access | Whisper モデル取得、Groq リクエスト、gTTS 音声合成 |

## インストール

次の手順でプロジェクトをセットアップします。

1. **リポジトリをクローン:**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **依存関係をインストール:**

必要な Python ライブラリをインストールします。

```bash
pip install -r requirements.txt
```

また、Google Colab を使用している場合は以下でインストールできます。

```python
!pip install gradio groq-api openai-whisper gtts
```

## 設定

### Groq API キーの設定

Groq API キーを環境変数に追加します。

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Google Colab では次のように設定できます。

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### 重要な実行時メモ（現在のコード挙動）

現在のスクリプトでは、Groq クライアントがハードコードされたプレースホルダーで初期化されています。

```python
client = Groq(api_key="your_groq_api_key")
```

環境変数 `GROQ_API_KEY` だけを設定している場合は、スクリプトを `os.environ` から読み取る実装に更新するか、プレースホルダーを直接置き換えてください。そうしないと API 認証に失敗します。

## 使い方

チャットボットを起動するには、メインスクリプトを実行します。

```bash
python voice_to_voice_chatbot.py
```

または Google Colab の場合:

スクリプトをコードセルにコピーして実行します。

Gradio インターフェースが起動し、チャットボットと対話できるようになります。

### チャットボットとの対話

- **Using Microphone**: マイクに直接話しかけます。チャットボットが音声を文字起こしし、応答を生成して音声として再生します。
- **Uploading Audio**: 事前録音した音声ファイルをアップロードします。チャットボットが音声を文字起こしし、応答を生成して再び音声化します。

## 例

### 音声フローの例

1. 録音: "What are three tips to learn Python quickly?"
2. Whisper がプロンプトテキストを文字起こし。
3. Groq の LLaMA モデルが回答を生成。
4. gTTS が MP3 応答を生成。
5. Gradio が応答テキストと音声再生を表示。

### 実行コマンド例

```bash
python voice_to_voice_chatbot.py
```

想定結果: ブラウザでローカルの Gradio アプリが開き、1 つの音声入力と 2 つの出力（テキスト + 音声）が表示されます。

## 開発メモ

- メインのパイプライン関数: `chatbot_pipeline(audio_path)`。
- Whisper モデルは起動時に読み込み: `whisper.load_model("base")`。
- 一時 MP3 ファイルは `NamedTemporaryFile(..., delete=False)` で作成。
- 現在のエラーハンドリングは UI に `(str(e), None)` を返す実装。
- `requirements.txt` には `whisper` と `openai-whisper` の両方が含まれており、環境によっては冗長な可能性があります。

## トラブルシューティング

### よくある問題

`ModuleNotFoundError`: 次のコマンドで Whisper モジュールの正しいバージョンをインストールしているか確認してください。

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: API キーを再確認し、環境変数に正しく設定されていることを確認してください。

追加のトラブルシューティング:

- アプリ起動直後に認証エラーで失敗する場合は、`voice_to_voice_chatbot.py` 内のハードコードされた `api_key="your_groq_api_key"` を確認してください。
- マイク入力が使えない場合は、まず音声ファイルをアップロードして STT -> LLM -> TTS パイプラインの動作を検証してください。
- 応答音声が空の場合は、gTTS への外向きネットワーク接続を確認してください。

### クイック診断チェックリスト

| チェック | 検証内容 |
|---|---|
| API key wiring | `Groq(api_key=...)` がプレースホルダーのままになっていない |
| Whisper install | `openai-whisper` を問題なく import できる |
| Network path | Groq + gTTS への外向きアクセスが可能 |
| Audio source | マイク権限が有効、またはファイルアップロードが動作 |

## ロードマップ

- 既定で Groq API キーを環境変数から直接読み込む。
- パイプラインのヘルパー関数向けテストを追加する。
- 環境変数または CLI 引数による任意のモデル/設定を追加する。
- 言語ナビゲーションリンクに対応する i18n README ファイルを `i18n/` 配下に追加する。
- デプロイ手段（例: Docker や Hugging Face Spaces）を追加する。

## コントリビュート

コントリビューションを歓迎します。リポジトリをフォークし、新しいブランチを作成して、変更内容のプルリクエストを送ってください。

推奨される貢献フロー:

1. リポジトリをフォークしてクローンする。
2. 機能ブランチを作成する。
3. 変更を実装してテストする。
4. 内容を明確に説明したプルリクエストを作成する。

## サポート

現在のリポジトリ内容では、寄付/スポンサー向けリンクは明示的に見つかりませんでした。メンテナーがサポート窓口を追加した場合は、ここに記載してください。

## ライセンス

このプロジェクトは MIT License の下で提供されます。詳細は LICENSE ファイルを参照してください。

前提: `LICENSE` ファイルは意図されているものの、このリポジトリのスナップショットには未追加の可能性があります。
