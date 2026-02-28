[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


# Chatbot Giọng nói sang Giọng nói sử dụng Whisper, LLaMA và Groq API

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Dự án này là một chatbot giọng nói sang giọng nói theo thời gian thực, sử dụng Whisper của OpenAI để chuyển giọng nói thành văn bản, LLaMA 8B thông qua Groq API để tạo phản hồi, và Google Text-to-Speech (gTTS) để chuyển văn bản trở lại thành giọng nói. Giao diện chatbot được xây dựng bằng Gradio, cho phép người dùng tương tác với bot bằng cách nói trực tiếp hoặc tải tệp âm thanh lên.

## Tổng quan

Ứng dụng triển khai một vòng hội thoại âm thanh hoàn chỉnh trong một script Python duy nhất:

1. Nhận âm thanh người dùng từ microphone hoặc tệp tải lên.
2. Chuyển giọng nói thành văn bản bằng Whisper (mô hình `base`).
3. Tạo phản hồi qua Groq (`llama3-8b-8192`).
4. Chuyển văn bản phản hồi thành âm thanh MP3 bằng gTTS.
5. Trả về cả văn bản phản hồi và âm thanh có thể phát trong giao diện web Gradio.

### Quy trình hội thoại

| Giai đoạn | Thành phần | Đầu ra |
|---|---|---|
| 🎙️ Đầu vào | Gradio Audio (mic/file) | Đường dẫn âm thanh |
| 📝 Phiên âm | Whisper `base` | Văn bản người dùng |
| 🧠 Suy luận | Groq + `llama3-8b-8192` | Văn bản trợ lý |
| 🔊 Tổng hợp | gTTS | Phản hồi MP3 |
| 🖥️ Phân phối | Giao diện Gradio | Văn bản + âm thanh phát được |

## Tính năng

- **Speech-to-Text**: Chuyển ngôn ngữ nói thành văn bản bằng mô hình Whisper của OpenAI.
- **Phản hồi do AI tạo**: Dùng LLaMA 8B thông qua Groq API để tạo phản hồi thông minh dựa trên văn bản đã phiên âm.
- **Text-to-Speech**: Chuyển các phản hồi văn bản được tạo thành giọng nói bằng Google Text-to-Speech (gTTS).
- **Tương tác thời gian thực**: Chatbot hoạt động theo thời gian thực, cho phép người dùng tương tác qua microphone hoặc tải tệp âm thanh lên qua giao diện web.
- **Runtime đơn giản trong một tệp**: Toàn bộ pipeline chatbot được triển khai trong `voice_to_voice_chatbot.py` để dễ thử nghiệm.

## Cấu trúc dự án

```text
Voice-to-text-and-voice-chatbot/
├── voice_to_voice_chatbot.py               # Script chính để chạy chatbot
├── requirements.txt                        # Danh sách phụ thuộc Python
├── README.md                               # Tài liệu dự án (tiếng Anh)
├── i18n/                                   # README bản địa hóa (được lên kế hoạch/tạo trong pipeline)
└── .auto-readme-work/20260228_230442/      # Ngữ cảnh/artefact tự động hóa README
```

Tham chiếu cấu trúc cũ từ README chuẩn:

```text
voice-to-voice-chatbot/
├── voice_to_voice_chatbot.py  # Script chính để chạy chatbot
├── requirements.txt           # Danh sách phụ thuộc Python
├── README.md                  # Tài liệu dự án
└── .gitignore                 # Tệp Git ignore
```

## Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn đã đáp ứng các yêu cầu sau:

- Đã cài Python 3.7 trở lên trên máy cục bộ hoặc Google Colab.
- Có Groq API key. Bạn có thể đăng ký API key [tại đây](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) hoặc môi trường Python cục bộ đã cài các thư viện cần thiết.
- Có kết nối Internet để:
  - Tải mô hình Whisper lần đầu.
  - Gọi Groq API.
  - Tạo âm thanh gTTS.

### Tóm tắt yêu cầu

| Yêu cầu | Lý do cần thiết |
|---|---|
| Python `3.7+` | Runtime cho script chatbot và các phụ thuộc |
| Groq API Key | Quyền truy cập xác thực vào suy luận mô hình LLaMA |
| Colab hoặc Local Env | Môi trường chạy cho Gradio + thư viện ML |
| Internet Access | Tải mô hình Whisper, yêu cầu Groq, tổng hợp gTTS |

## Cài đặt

Làm theo các bước sau để thiết lập dự án:

1. **Clone Repository:**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Cài đặt Dependencies:**

Cài các thư viện Python cần thiết:

```bash
pip install -r requirements.txt
```

Hoặc nếu bạn dùng Google Colab, bạn có thể cài thư viện bằng:

```python
!pip install gradio groq-api openai-whisper gtts
```

## Cấu hình

### Thiết lập Groq API Key

Thêm Groq API key vào biến môi trường:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Trong Google Colab, bạn có thể đặt API key bằng:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Lưu ý runtime quan trọng (hành vi mã hiện tại)

Script hiện tại khởi tạo Groq client bằng một placeholder hardcoded:

```python
client = Groq(api_key="your_groq_api_key")
```

Nếu bạn chỉ đặt `GROQ_API_KEY` trong môi trường, hãy cập nhật script để đọc từ `os.environ` hoặc thay placeholder trực tiếp, nếu không xác thực API sẽ thất bại.

## Cách sử dụng

Để khởi động chatbot, chạy script chính:

```bash
python voice_to_voice_chatbot.py
```

Hoặc trên Google Colab:

Sao chép script vào một ô code rồi thực thi.

Giao diện Gradio sẽ khởi chạy, cho phép bạn tương tác với chatbot.

### Tương tác với chatbot

- **Dùng Microphone**: Nói trực tiếp vào microphone. Chatbot sẽ phiên âm lời nói của bạn, tạo phản hồi và phát lại dưới dạng âm thanh.
- **Tải tệp âm thanh lên**: Tải lên một tệp âm thanh đã ghi sẵn. Chatbot sẽ phiên âm âm thanh, tạo phản hồi và chuyển phản hồi thành giọng nói.

## Ví dụ

### Ví dụ luồng giọng nói

1. Ghi âm: "What are three tips to learn Python quickly?"
2. Whisper phiên âm lời nhắc của bạn thành văn bản.
3. Mô hình Groq LLaMA tạo câu trả lời.
4. gTTS tạo phản hồi MP3.
5. Gradio hiển thị văn bản phản hồi và phần phát âm thanh.

### Ví dụ lệnh chạy

```bash
python voice_to_voice_chatbot.py
```

Kết quả mong đợi: một ứng dụng Gradio cục bộ mở trong trình duyệt với một đầu vào âm thanh và hai đầu ra (text + audio).

## Ghi chú phát triển

- Hàm pipeline chính: `chatbot_pipeline(audio_path)`.
- Mô hình Whisper được tải khi khởi động: `whisper.load_model("base")`.
- Các tệp MP3 tạm được tạo bằng `NamedTemporaryFile(..., delete=False)`.
- Xử lý lỗi hiện tại trả về `(str(e), None)` cho giao diện.
- Các phụ thuộc trong `requirements.txt` gồm cả `whisper` và `openai-whisper`; điều này có thể dư thừa tùy môi trường.

## Khắc phục sự cố

### Các vấn đề thường gặp

`ModuleNotFoundError`: Hãy đảm bảo bạn đã cài đúng phiên bản module Whisper bằng

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: Kiểm tra lại API key của bạn và đảm bảo nó được đặt đúng trong biến môi trường.

Khắc phục bổ sung:

- Nếu ứng dụng lỗi ngay với lỗi xác thực, hãy kiểm tra `api_key="your_groq_api_key"` hardcoded trong `voice_to_voice_chatbot.py`.
- Nếu không thể thu âm từ microphone, hãy tải lên tệp âm thanh trước để xác thực pipeline STT -> LLM -> TTS.
- Nếu âm thanh phản hồi bị trống, hãy xác nhận quyền truy cập mạng đi ra cho gTTS.

### Checklist chẩn đoán nhanh

| Kiểm tra | Xác thực |
|---|---|
| Kết nối API key | `Groq(api_key=...)` không để placeholder |
| Cài đặt Whisper | `openai-whisper` import thành công |
| Đường mạng | Có truy cập đi ra cho Groq + gTTS |
| Nguồn âm thanh | Quyền mic đã bật hoặc tải tệp lên hoạt động |

## Lộ trình

- Mặc định đọc Groq API key trực tiếp từ biến môi trường.
- Thêm test cho các hàm helper của pipeline.
- Thêm tùy chọn model/cấu hình qua biến môi trường hoặc CLI args.
- Thêm các tệp README i18n trong `i18n/` khớp với các liên kết điều hướng ngôn ngữ.
- Thêm tùy chọn triển khai (ví dụ: Docker hoặc Hugging Face Spaces).

## Đóng góp

Mọi đóng góp đều được chào đón! Hãy fork repository này, tạo một branch mới và gửi pull request với các thay đổi của bạn.

Quy trình đóng góp được gợi ý:

1. Fork và clone repository.
2. Tạo feature branch.
3. Thực hiện và kiểm thử các thay đổi.
4. Mở pull request với mô tả rõ ràng.

## Hỗ trợ

Không tìm thấy liên kết quyên góp/tài trợ rõ ràng trong nội dung repository hiện tại. Nếu maintainers thêm các kênh hỗ trợ, chúng nên được liệt kê ở đây.

## Giấy phép

Dự án này được cấp phép theo MIT License. Xem tệp LICENSE để biết chi tiết.

Giả định: tệp `LICENSE` được dự định có nhưng có thể chưa xuất hiện trong snapshot repository này.
