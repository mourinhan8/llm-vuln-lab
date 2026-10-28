# Bài thực hành An ninh AI (LLM Vuln Lab)

Nền tảng thực hành an ninh LLM tương tác dành cho sinh viên, lập trình viên, kỹ sư an ninh và đội ngũ sản phẩm AI. Nền tảng minh hoạ các rủi ro phổ biến trong ứng dụng LLM qua mô phỏng thực hành, bao gồm tấn công chèn chỉ dẫn qua hội thoại, tấn công RAG, xử lý đầu ra không an toàn, lỗ hổng API/middleware, lạm dụng agent/tool và bài tập mô hình hoá mối đe doạ AI.

Dự án được xây dựng nhẹ dưới dạng HTML + FastAPI để chạy nội bộ trên máy tính cá nhân hoặc phòng thực hành mà không cần hạ tầng đám mây phức tạp.

> Chỉ dùng cho giáo dục. Không triển khai lab này lên máy chủ công khai.

# Trang web mẫu

https://crazywifi.github.io/LLM_Vulnerable_lab/llm_vuln_lab_UI.html

> Lưu ý: trang mẫu trực tuyến có thể cũ hơn bản trong kho mã. Để thực hành đầy đủ, chạy backend FastAPI nội bộ để các mô phỏng API và middleware hoạt động.

## Lưu ý quan trọng về chọn model

Không phải model nào cũng phản hồi giống nhau trong lab này. Các model thương mại có rào cản an toàn mạnh (GPT, Claude, Gemini…) có thể từ chối nhiều lệnh tấn công ngay cả khi chạy ở chế độ Có lỗ hổng. Điều này không có nghĩa lab bị lỗi — mà model đã có cơ chế bảo vệ riêng chặn trước khi lab prompt thể hiện hành vi dễ bị tấn công.

Để minh hoạ rõ nhất trong lớp học, dùng model nội bộ hoặc model nghiên cứu ít rào cản hơn. Ví dụ thường phù hợp cho thực hành an ninh, khi có qua LM Studio, Ollama hoặc OpenRouter:

- `meta-llama-3.1-8b-instruct-abliterated`
- `cognitivecomputations/dolphin-mistral-24b-venice-edition`
- `dolphin-llama3`
- `dolphin-mixtral`
- `nous-hermes`
- Các model instruct "abliterated", "uncensored" hoặc hướng nghiên cứu an ninh khác

Model có sẵn thay đổi theo nhà cung cấp và tên có thể khác. Luôn chọn model có trách nhiệm và chỉ dùng cho giáo dục an ninh được uỷ quyền.

Cũng lưu ý rằng payload không đảm bảo thành công mọi lần. Một payload đã thành công trước đó có thể thất bại sau vì:

- Đầu ra LLM mang tính xác suất và có thể thay đổi giữa các lần chạy.
- Model có thể thận trọng hơn sau các tin nhắn trước trong cùng hội thoại.
- Lịch sử hội thoại có thể thay đổi cách model hiểu câu hỏi tiếp theo.
- Bộ lọc an toàn hoặc phiên bản model từ nhà cung cấp có thể thay đổi.
- Nhiệt độ, độ dài ngữ cảnh và thứ tự system prompt có thể ảnh hưởng hành vi.

Nếu payload không còn hiệu quả, nhấn **Xoá hội thoại** và thử lại từ đầu. Việc này đặt lại lịch sử hội thoại nội bộ và thường làm cho minh hoạ ổn định hơn.

## Lab dạy gì

Lab giúp người học hiểu:

- Cách hệ thống LLM bị tấn công qua prompt, tài liệu, công cụ, API và đầu ra render.
- Tại sao system prompt và văn bản rào cản không phải ranh giới an toàn hoàn chỉnh.
- Hành vi dễ bị tấn công khác với hành vi có phòng vệ như thế nào.
- Cách pipeline RAG bị đầu độc bởi nội dung truy xuất không đáng tin cậy.
- Cách quy trình agent có thể thất bại khi công cụ có quá nhiều quyền.
- Cách middleware không an toàn biến tính năng LLM thành rủi ro web, API hoặc hạ tầng.
- Cách mô hình hoá mối đe doạ hệ thống AI dùng bề mặt tấn công, tác nhân, kỹ thuật và biện pháp kiểm soát.

## Tính năng chính

- Giao diện trình duyệt đơn trang cho bài thực hành tương tác.
- Backend FastAPI cho mô phỏng tấn công API và middleware.
- Hỗ trợ nhà cung cấp LLM nội bộ và đám mây:
  - Ollama
  - LM Studio
  - OpenRouter
- Chế độ Có lỗ hổng và Có phòng vệ để so sánh hành vi không an toàn và an toàn hơn.
- Bài thực hành dạng hội thoại cho tấn công qua chat.
- Bài thực hành dạng API với JSON request body và headers có thể chỉnh sửa.
- Mô phỏng RAG cho đầu độc, thu nhập, metadata, chỉ mục cũ, nhầm lẫn nguồn và rò rỉ ngữ cảnh.
- Mô phỏng middleware cho SSRF, giới hạn tốc độ, rò rỉ streaming, đầu độc bộ nhớ, lạm dụng chi phí, tiết lộ lỗi và rò rỉ phiên người dùng khác.
- Bài tập mô hình hoá mối đe doạ ATLAS và thiết kế an toàn.

## File trong kho mã

| File | Mục đích |
| --- | --- |
| `llm_vuln_lab_UI.html` | Giao diện trình duyệt chính và định nghĩa bài thực hành |
| `lab_backend_server.py` | Backend FastAPI, proxy LLM và các endpoint mô phỏng |
| `requirements.txt` | Phụ thuộc Python |
| `start.sh` | Trình khởi động macOS/Linux |
| `start.bat` | Trình khởi động Windows |

## Chế độ Có lỗ hổng và Có phòng vệ

Nền tảng có hai chế độ cho mỗi bài.

### Chế độ Có lỗ hổng

Chế độ có lỗ hổng cố ý dùng prompt không an toàn, giả định yếu hoặc middleware mô phỏng không an toàn. Nó được thiết kế để minh hoạ cách một cuộc tấn công có thể thành công.

Ví dụ:

- Chatbot có thể tiết lộ bí mật từ system prompt.
- Trợ lý RAG có thể tuân theo chỉ dẫn ẩn trong tài liệu truy xuất.
- Endpoint middleware có thể tin tưởng `role: "admin"` từ JSON do client kiểm soát.
- Trình render đầu ra có thể cho phép HTML không an toàn.
- Agent có thể tuân theo chỉ dẫn độc hại từ đầu ra công cụ.

### Chế độ Có phòng vệ

Chế độ có phòng vệ minh hoạ hành vi an toàn hơn bằng cách áp dụng các biện pháp kiểm soát như:

- Từ chối yêu cầu ghi đè chỉ dẫn.
- Xử lý tài liệu truy xuất và đầu ra công cụ như dữ liệu không đáng tin cậy.
- Không tiết lộ system prompt hoặc bí mật.
- Xác thực đầu vào API.
- Bỏ qua trường xác thực do client cung cấp.
- Giới hạn phiên bản model trong danh sách cho phép.
- Chặn mẫu lệnh, SQL, HTML và sử dụng công cụ nguy hiểm.
- Thực thi quyền tối thiểu cho agent.

Chế độ có phòng vệ không đảm bảo an toàn hoàn chỉnh. Nó là so sánh giáo dục, minh hoạ các mẫu thiết kế an toàn hơn.

## Độ phủ bài kiểm thử

Lab bao gồm các danh mục sau.

Danh sách bài thực hành nội bộ hiện tại:

- 55 kịch bản bài thực hành
- 32 bài chat/hành vi model
- 21 bài mô phỏng API và middleware
- 2 bài ATLAS/thiết kế kiểm soát

### Chèn chỉ dẫn (Prompt Injection)

- Chèn chỉ dẫn trực tiếp
- Chèn chỉ dẫn gián tiếp
- Chèn chỉ dẫn nhiều lượt (Crescendo)
- Buôn lậu chỉ dẫn / Payload mã hoá
- Chèn chỉ dẫn qua RAG
- Chèn chỉ dẫn qua HTTP Headers
- Chèn chỉ dẫn đa phương thức qua OCR/metadata

### Vượt rào và Rò rỉ chỉ dẫn hệ thống

- Vượt rào qua đóng vai / DAN
- Vượt rào qua bối cảnh hư cấu
- Vượt rào qua dịch thuật
- Vượt rào Many-Shot
- Tấn công Crescendo
- Trích xuất chỉ dẫn hệ thống
- Rò rỉ chỉ dẫn qua thiết kế prompt không an toàn

### Tấn công pipeline RAG

- Đầu độc RAG / Chiếm đoạt truy xuất
- Nhồi khoảng cách ngữ cảnh
- Chèn chỉ dẫn liên tài liệu
- Đầu độc thu nhập RAG qua text/PDF/DOCX ẩn
- Chèn chỉ dẫn vào metadata RAG qua title, author, tag
- Chiếm đoạt truy xuất RAG qua nhồi từ khóa
- Chỉ mục RAG cũ / Dữ liệu bị thu hồi
- Nhầm lẫn nguồn RAG và thất bại ranh giới tin cậy
- Rò rỉ ngữ cảnh RAG qua raw chunks

### Vượt bộ lọc và an toàn

- Vượt bộ lọc từ khoá
- Vượt bộ lọc đầu ra
- Vượt kiểm duyệt qua ý đồ phân tán
- Ghi đè chỉ dẫn hệ thống
- Vượt rào qua giả danh quyền lực

### Rò rỉ dữ liệu và quyền riêng tư

- Rò rỉ PII
- Tiết lộ PHI
- Rò rỉ API key / Bí mật
- Tiết lộ logic nghiệp vụ
- Tái tạo dữ liệu huấn luyện
- Suy luận thành viên
- Suy luận thuộc tính
- Đầu độc bộ nhớ
- Rò rỉ phiên người dùng khác
- Rò rỉ phản hồi streaming

### Xử lý đầu ra không an toàn

- XSS qua đầu ra LLM
- SQL Injection qua LLM
- Command Injection qua LLM
- SSRF qua truy xuất URL kiểu LLM
- Chèn định dạng / Parser Injection
- Chèn liên kết Markdown / Hình ảnh

### Khai thác Plugin, Tool và Agent

- Lạm dụng công cụ
- Lỗ hổng chuỗi Agent
- Quyền quá mức
- Chèn chỉ dẫn trong đầu ra công cụ
- TOCTOU trong luồng Agent
- Plugin SSRF / Lạm dụng truy xuất URL
- SSRF ra ngoài qua công cụ HTTP của model

### Tấn công API và Middleware

- Lạm dụng API / Vượt giới hạn tốc độ
- Thao túng yêu cầu
- Vượt xác thực
- Nhầm lẫn phiên bản model
- Chèn chỉ dẫn qua HTTP Headers
- Tiết lộ lỗi backend
- Lạm dụng chi phí Token / Từ chối ví
- Rò rỉ phiên người dùng khác (IDOR)
- Rò rỉ token streaming trước khi lọc

### Khai niệm chuỗi cung ứng và thao túng model

- Mô phỏng chèn backdoor
- Mô phỏng thao túng chuỗi cung ứng
- Mô phỏng hành vi đầu độc dữ liệu / fine-tune
- Khai niệm rủi ro plugin bên thứ ba qua bài agent/tool
- Thảo luận ảo giác package và toàn vẹn model qua bài chuỗi cung ứng

### Mô hình hoá mối đe doạ và thiết kế an toàn

- Liệt kê bề mặt tấn công
- Hồ sơ tác nhân đe doạ
- Ánh xạ kỹ thuật ATLAS
- Xác thực và làm sạch đầu vào
- Lọc đầu ra
- Củng cố chỉ dẫn hệ thống
- Rào cản ngữ nghĩa
- Giám sát và phát hiện bất thường
- Quyền tối thiểu cho Agent

## Cách chạy

### 1. Cài đặt phụ thuộc Python

```bash
pip install -r requirements.txt
```

### 2. Khởi động nhà cung cấp LLM

Có thể dùng một trong các lựa chọn sau.

#### Lựa chọn A: Ollama

Cài đặt Ollama, sau đó chạy:

```bash
ollama pull llama3.2
ollama serve
```

Nếu trình duyệt hoặc cài đặt nội bộ cần CORS:

```bash
OLLAMA_ORIGINS=* ollama serve
```

#### Lựa chọn B: LM Studio

1. Cài đặt LM Studio.
2. Tải hoặc nạp một model chat.
3. Mở thẻ Local Server.
4. Bắt đầu máy chủ.
5. Bật CORS nếu cần.

#### Lựa chọn C: OpenRouter

1. Tạo API key từ OpenRouter.
2. Thêm vào giao diện cài đặt trong UI, hoặc đặt biến môi trường:

```bash
export OPENROUTER_API_KEY="sk-or-v1-your-key"
```

Trên Windows PowerShell:

```powershell
$env:OPENROUTER_API_KEY="sk-or-v1-your-key"
```

### 3. Khởi động backend lab

```bash
python lab_backend_server.py
```

Hoặc dùng trình khởi động:

```bash
# Windows
start.bat

# macOS/Linux
bash start.sh
```

### 4. Mở giao diện

Mở:

```text
http://localhost:8000
```

Giao diện được phục vụ bởi backend FastAPI. Đây là cách chạy lab được khuyến nghị vì các mô phỏng API/middleware cần endpoint backend.

## Cách sử dụng cơ bản

1. Chọn một bài từ bảng bên trái.
2. Chọn chế độ **Có lỗ hổng**.
3. Thử một trong các gợi ý hoặc viết lệnh tấn công riêng.
4. Quan sát hành vi của model hoặc endpoint mô phỏng.
5. Chuyển sang chế độ **Có phòng vệ**.
6. Chạy lại cùng kiểm thử.
7. So sánh sự khác biệt và thảo luận biện pháp kiểm soát nào giảm thiểu rủi ro.

Đối với bài API, giao diện sẽ hiển thị:

- Đường dẫn endpoint.
- Request headers có thể chỉnh sửa.
- JSON body có thể chỉnh sửa.
- Nút chạy mô phỏng.
- Kết quả mô phỏng an ninh trả về.

Bài API không cần model LLM trực tiếp. Chúng dùng backend FastAPI để trả về hành vi dễ bị tấn công và có phòng vệ xác định, phù hợp cho minh hoạ trong lớp học.

Đối với bài ATLAS/thiết kế, giao diện hiển thị:

- Tình huống.
- Bề mặt tấn công.
- Tác nhân đe doạ.
- Ánh xạ kỹ thuật.
- Biện pháp phòng vệ.

## Luồng lớp học ví dụ

1. Bắt đầu với Chèn chỉ dẫn trực tiếp để thấy tại sao bí mật không thuộc system prompt.
2. Chuyển sang Chèn chỉ dẫn gián tiếp để thấy rủi ro từ tài liệu không đáng tin cậy.
3. Chạy Đầu độc RAG, Chèn metadata, Chiếm đoạt truy xuất, Nhầm lẫn nguồn và Rò rỉ ngữ cảnh để hiểu ranh giới tin cậy RAG.
4. Minh hoạ XSS, Chèn liên kết Markdown, SQL Injection, Command Injection và SSRF để kết nối rủi ro LLM với AppSec cổ điển.
5. Dùng Vượt giới hạn tốc độ, Vượt xác thực, Rò rỉ phiên người dùng khác, Rò rỉ streaming, Tiết lộ lỗi và Lạm dụng chi phí để dạy rủi ro API và nền tảng.
6. Dùng Lạm dụng công cụ, Chèn chỉ dẫn trong đầu ra công cụ, Lỗ hổng chuỗi Agent, Plugin SSRF và TOCTOU để dạy an ninh agent.
7. Kết thúc với Mô hình hoá mối đe doạ ATLAS và Kiểm soát thiết kế AI an toàn.

## Lưu ý sẵn sàng thực hành

- Bài chat cần nhà cung cấp LLM đã cấu hình: Ollama, LM Studio hoặc OpenRouter.
- Bài API và middleware là mô phỏng xác định và hoạt động mà không cần model LLM trực tiếp.
- Bài RAG là mô phỏng hành vi chat/model và cần nhà cung cấp LLM đã cấu hình.
- Bài ATLAS/thiết kế là bài tập xem xét xác định và hoạt động mà không cần model LLM trực tiếp.
- Lab cố ý chứa prompt dễ bị tấn công, bí mật giả và dữ liệu nội bộ giả. Chỉ dùng dữ liệu mẫu được cung cấp.
- Chế độ có phòng vệ là so sánh giáo dục, không phải framework an toàn production hoàn chỉnh.
- Nếu payload chat thất bại vì model từ chối, xoá hội thoại và thử model nội bộ/nghiên cứu yếu hơn cho minh hoạ.

## Công cụ này giúp đội ngũ hiểu điều gì

Công cụ này giúp đội ngũ xây dựng trực giác về:

- Cách lỗ hổng LLM xuất hiện trong ứng dụng thực tế.
- Tại sao an ninh AI vừa là an ninh model vừa là an ninh ứng dụng.
- Cách kẻ tấn công kết hợp điểm yếu prompt, dữ liệu, API và công cụ.
- Tại sao phòng vệ phải nhiều lớp xung quanh model.
- Cách chuyển phân loại rủi ro AI thành bài kiểm thử.
- Cách thảo luận an ninh LLM với lập trình viên dùng ví dụ cụ thể.
- Cách thiết kế tính năng AI an toàn hơn trước khi triển khai production.

## Ngoài phạm vi

Một số mục trong phân loại an ninh LLM đầy đủ không được triển khai như mô phỏng kỹ thuật thật vì cần nội bộ model, hạ tầng huấn luyện thật, cơ sở dữ liệu vector hoặc hệ thống chuỗi cung ứng nằm ngoài ứng dụng HTML/FastAPI nhẹ.

| Mục ngoài phạm vi | Lý do ngắn |
| --- | --- |
| Trọng số model độc hại | Cần tải, kiểm tra và xác thực file trọng số model thật |
| Backdoor embedding thật | Cần model embedding thật, đánh giá trigger và tìm kiếm vector |
| Đầu độc dataset đầy đủ | Cần pipeline huấn luyện hoặc thu nhập thật với thay đổi hành vi model đo được |
- Fine-tuning độc hại thật | Cần hạ tầng fine-tune, dataset, checkpoint và đánh giá |
| Lật nhãn | Cần dataset ML có giám sát và huấn luyện lại model |
| Đảo ngược model | Cần tấn công bảo mật toán học và truy cập logit/hành vi tự tin của model |
| Trích xuất model | Cần truy vấn khối lượng lớn và huấn luyện model thay thế |
| Tấn công chuyển giao thật | Cần nhiều model thật và đánh giá tấn công so sánh |
| Tấn công gradient hoặc tensor | Cần truy cập tensor, gradient, logit hoặc nội bộ model |
| Thao túng embedding thật | Cần vector embedding trực tiếp và hành vi tìm kiếm láng giềng gần nhất |
| Đầu độc truy xuất quy mô production | Lab mô phỏng đầu độc RAG và thất bại xếp hạng, nhưng không chạy vector database hay pipeline truy xuất production thật |
| Nhầm lẫn phụ thuộc | Cần registry package, pipeline build và mô phỏng giải quyết phụ thuộc |
| Xâm phạm chuỗi cung ứng vật lý | Cần nguồn gốc artifact thật, ký kết, triển khai hoặc luồng phân phối phần cứng/phần mềm |

Khi có thể, lab vẫn bao gồm mô phỏng khái niệm an toàn cho các chủ đề này, như hành vi backdoor chuỗi cung ứng, mô phỏng đầu độc fine-tune và đầu độc RAG.

## Cảnh báo an ninh

Kho mã này cố ý chứa prompt dễ bị tấn công và endpoint mô phỏng dễ bị tấn công. Giữ nội bộ.

Không:

- Triển khai backend này công khai.
- Đặt bí mật thật trong system prompt.
- Kết nối endpoint mô phỏng với hệ thống production.
- Dùng dữ liệu khách hàng, bệnh nhân, nhân viên hoặc thông tin xác thực thật.
- Cho bài agent/tool truy cập hệ thống file, email, đám mây hoặc cơ sở dữ liệu thật.

## Khắc phục sự cố

### Backend không khởi động

```bash
pip install -r requirements.txt
python lab_backend_server.py
```

### Cổng 8000 đã được dùng

```bash
python -m uvicorn lab_backend_server:app --host 0.0.0.0 --port 8001
```

Sau đó mở:

```text
http://localhost:8001
```

### Ollama không được phát hiện

- Đảm bảo Ollama đang chạy.
- Tải model bằng `ollama pull llama3.2`.
- Nếu cần, khởi động Ollama với CORS đã bật.

### LM Studio không được phát hiện

- Khởi động máy chủ nội bộ LM Studio.
- Xác nhận model đã được nạp.
- Bật CORS trong cài đặt máy chủ LM Studio.

### Bài API không hoạt động

Mở giao diện qua backend:

```text
http://localhost:8000
```

Không mở file HTML trực tiếp nếu cần mô phỏng API/middleware.

## Giấy phép và sử dụng

Lab này dành cho giáo dục được uỷ quyền, đào tạo an ninh và workshop red team AI nội bộ. Sử dụng có trách nhiệm và chỉ trong môi trường có quyền kiểm thử.