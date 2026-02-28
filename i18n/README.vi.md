[English](../README.md) · [العربية](README.ar.md) · [Español](README.es.md) · [Français](README.fr.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Tiếng Việt](README.vi.md) · [中文 (简体)](README.zh-Hans.md) · [中文（繁體）](README.zh-Hant.md) · [Deutsch](README.de.md) · [Русский](README.ru.md)


[![LazyingArt banner](https://github.com/lachlanchen/lachlanchen/raw/main/figs/banner.png)](https://github.com/lachlanchen/lachlanchen/blob/main/figs/banner.png)

# Chatbot giọng nói sang giọng nói sử dụng Whisper, LLaMA và Groq API

![Python](https://img.shields.io/badge/Python-3.7%2B-3776AB?logo=python&logoColor=white)
![Whisper](https://img.shields.io/badge/STT-OpenAI%20Whisper-1f6feb)
![Groq](https://img.shields.io/badge/LLM-Groq%20LLaMA%203--8B-FF6B35)
![gTTS](https://img.shields.io/badge/TTS-Google%20gTTS-34A853)
![UI](https://img.shields.io/badge/UI-Gradio-F97316)
![Status](https://img.shields.io/badge/Project-Active%20Prototype-2ea44f)

Đây là một chatbot giọng nói sang giọng nói theo thời gian thực, tận dụng Whisper của OpenAI cho nhận diện giọng nói thành văn bản, LLaMA 3 8B thông qua Groq API để tạo phản hồi, và Google Text-to-Speech (gTTS) để chuyển văn bản thành giọng nói. Giao diện dùng Gradio để người dùng có thể tương tác bằng giọng nói hoặc tải tệp âm thanh lên.

## Tổng quan

Ứng dụng triển khai một vòng hội thoại âm thanh đầy đủ trong một file Python duy nhất:

1. Nhận âm thanh người dùng từ micro hoặc tệp đã tải lên.
2. Chuyển văn bản lời nói bằng mô hình Whisper (`base`).
3. Tạo phản hồi qua Groq (`llama3-8b-8192`).
4. Chuyển văn bản phản hồi thành MP3 bằng gTTS.
5. Trả về cả văn bản phản hồi và âm thanh phát được trong giao diện web Gradio.

### Quy trình hội thoại

| Giai đoạn | Thành phần | Đầu ra |
|---|---|---|
| 🎙️ Đầu vào | Gradio Audio (mic/file) | Đường dẫn âm thanh |
| 📝 Phiên âm | Whisper `base` | Văn bản người dùng |
| 🧠 Lập luận | Groq + `llama3-8b-8192` | Văn bản trợ lý |
| 🔊 Tổng hợp | gTTS | Phản hồi MP3 |
| 🖥️ Phân phối | Gradio UI | Văn bản + âm thanh có thể phát |

## ⭐ Tính năng

- **Speech-to-Text**: Chuyển lời nói sang văn bản bằng mô hình Whisper của OpenAI.
- **Phản hồi do AI tạo**: Sử dụng LLaMA 8B qua Groq API để sinh phản hồi thông minh từ văn bản đã phiên âm.
- **Text-to-Speech**: Chuyển văn bản phản hồi về lại giọng nói bằng Google Text-to-Speech (gTTS).
- **Tương tác thời gian thực**: Tương tác qua micro hoặc tải lên tệp âm thanh qua giao diện web.
- **Runtime đơn giản một file**: Toàn bộ pipeline chatbot được cài trong `voice_to_voice_chatbot.py`.
- **Tài liệu đa ngôn ngữ**: README tiếng Anh chính có các liên kết đến bản dịch tại `i18n/`.

## 📁 Cấu trúc dự án

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

## ✅ Điều kiện tiên quyết

Trước khi bắt đầu, hãy đảm bảo bạn đã đáp ứng các yêu cầu sau:

- Đã cài Python 3.7 trở lên trên máy cục bộ hoặc Google Colab.
- Có khóa API Groq. Bạn có thể đăng ký khóa tại [Groq](https://groq.com/).
- [Google Colab](https://colab.research.google.com/) hoặc môi trường Python cục bộ có đủ thư viện cần thiết.
- Kết nối internet cho:
  - Tải model Whisper lần đầu.
  - Gọi API Groq.
  - Tạo âm thanh bằng gTTS.

### Yêu cầu tóm tắt

| Yêu cầu | Lý do cần thiết |
|---|---|
| Python `3.7+` | Runtime cho script chatbot và các dependencies |
| Groq API Key | Truy cập có xác thực vào suy luận model LLaMA |
| Colab hoặc môi trường cục bộ | Môi trường chạy Gradio + thư viện ML |
| Kết nối Internet | Tải model Whisper, gọi Groq, tổng hợp gTTS |

## 🛠️ Cài đặt

Làm theo các bước sau để thiết lập dự án:

1. **Clone Repository**

```bash
git clone https://github.com/aquibali01/voice-to-voice-chatbot.git
cd voice-to-voice-chatbot
```

2. **Cài đặt dependencies**

Cài các thư viện Python cần thiết:

```bash
pip install -r requirements.txt
```

Hoặc trong Google Colab:

```python
!pip install gradio groq-api openai-whisper gtts
```

## ⚙️ Cấu hình

### Thiết lập Groq API Key

Xuất Groq API key của bạn:

```bash
export GROQ_API_KEY='your_groq_api_key'
```

Trong Google Colab, hãy đặt trực tiếp trong runtime:

```python
import os
os.environ['GROQ_API_KEY'] = 'your_groq_api_key'
```

### Lưu ý quan trọng về runtime (hành vi mã hiện tại)

Script hiện tại khởi tạo client Groq với một placeholder cố định:

```python
client = Groq(api_key="your_groq_api_key")
```

Nếu bạn chỉ đặt `GROQ_API_KEY` trong môi trường, hãy cập nhật script để đọc từ `os.environ` (hoặc thay trực tiếp placeholder), nếu không các cuộc gọi xác thực có thể thất bại.

## ▶️ Sử dụng

Để bắt đầu chatbot, chạy:

```bash
python voice_to_voice_chatbot.py
```

Hoặc trong Google Colab:

Copy script vào một ô code và thực thi.

Giao diện Gradio sẽ khởi chạy cục bộ, cho phép bạn tương tác với chatbot.

### Tương tác với chatbot

- **Dùng micro**: Nói trực tiếp vào micro. Chatbot sẽ phiên âm giọng nói của bạn, tạo phản hồi và phát lại dưới dạng âm thanh.
- **Tải lên âm thanh**: Tải lên một tệp âm thanh đã thu trước. Chatbot sẽ phiên âm bản ghi, tạo phản hồi và phát âm thanh đã tổng hợp.

## 🎬 Ví dụ

### Ví dụ luồng giọng nói

1. Ghi âm: "What are three tips to learn Python quickly?"
2. Whisper phiên âm prompt của bạn thành văn bản.
3. Groq LLaMA tạo câu trả lời.
4. gTTS sinh phản hồi MP3.
5. Gradio hiển thị văn bản phản hồi và trình phát âm thanh.

### Ví dụ lệnh chạy

```bash
python voice_to_voice_chatbot.py
```

Kết quả mong đợi: một ứng dụng Gradio cục bộ mở trong trình duyệt với một đầu vào âm thanh và hai đầu ra (text + audio).

## 🧪 Ghi chú phát triển

- Hàm pipeline chính: `chatbot_pipeline(audio_path)`.
- Model Whisper được nạp khi khởi động với `whisper.load_model("base")`.
- Các tệp MP3 tạm được tạo bằng `NamedTemporaryFile(..., delete=False)`.
- Xử lý lỗi hiện tại trả về `(str(e), None)` về UI.
- `requirements.txt` hiện bao gồm cả `whisper` và `openai-whisper`; có thể bị thừa tùy theo môi trường.

## 🐞 Khắc phục sự cố

### Vấn đề thường gặp

`ModuleNotFoundError`: Đảm bảo đã cài đúng package Whisper:

```python
!pip install -U openai-whisper
```

`Groq API Key Error`: Kiểm tra lại sự sẵn có và tính hợp lệ của key trong môi trường hoặc script của bạn.

Xử lý sự cố bổ sung:

- Nếu app lỗi xác thực, xác minh `api_key="your_groq_api_key"` cứng trong `voice_to_voice_chatbot.py`.
- Nếu không thu được microphone, hãy tải một tệp âm thanh trước để kiểm tra lại pipeline STT → LLM → TTS.
- Nếu âm thanh phản hồi trống, hãy xác nhận quyền truy cập mạng đầu ra cho gTTS và Groq.

### Danh sách chẩn đoán nhanh

| Kiểm tra | Xác minh |
|---|---|
| Cấu hình API key | `Groq(api_key=...)` không để nguyên placeholder |
| Cài đặt Whisper | `openai-whisper` import thành công |
| Đường dẫn mạng | Có truy cập outbound cho Groq + gTTS |
| Nguồn âm thanh | Quyền micro đã bật hoặc upload tệp hoạt động |

## 🗺️ Lộ trình

- Đọc trực tiếp Groq API key từ biến môi trường theo mặc định.
- Thêm tests cho các hàm tiện ích pipeline.
- Thêm tùy chọn model/cấu hình tùy ý qua biến môi trường hoặc tham số CLI.
- Thêm tuỳ chọn triển khai (ví dụ Docker hoặc Hugging Face Spaces).

## ❤️ Support

| Donate | PayPal | Stripe |
|---|---|---|
| [![Donate](https://img.shields.io/badge/Donate-LazyingArt-0EA5E9?style=for-the-badge&logo=ko-fi&logoColor=white)](https://chat.lazying.art/donate) | [![PayPal](https://img.shields.io/badge/PayPal-RongzhouChen-00457C?style=for-the-badge&logo=paypal&logoColor=white)](https://paypal.me/RongzhouChen) | [![Stripe](https://img.shields.io/badge/Stripe-Donate-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://buy.stripe.com/aFadR8gIaflgfQV6T4fw400) |

## 🤝 Đóng góp

Các đóng góp đều được hoan nghênh. Vui lòng fork repository này, tạo nhánh mới, và gửi pull request với thay đổi của bạn.

Quy trình đóng góp gợi ý:

1. Fork và clone repository.
2. Tạo một feature branch.
3. Triển khai và kiểm thử thay đổi.
4. Mở pull request kèm mô tả rõ ràng và lý do.

## 📄 Giấy phép

Dự án này hiện được ghi nhận là cấp phép theo MIT. Xem file LICENSE để biết chi tiết.

Giả định: tệp `LICENSE` được dự định có nhưng có thể chưa có trong snapshot kho này.
