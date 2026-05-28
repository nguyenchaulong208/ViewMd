Dưới đây là toàn bộ nội dung tài liệu PRD được định dạng Markdown chuẩn chỉnh, viết bằng tiếng Việt để bạn dễ dàng làm việc với các AI Coding Agent. Bạn chỉ cần sao chép toàn bộ nội dung trong ô mã nguồn bên dưới:

```markdown
# THÔNG SỐ KỸ THUẬT & TÀI LIỆU YÊU CẦU SẢN PHẨM (PRD)
## Dự án: Personal AI Reminder Agent (Phiên bản MVP cho Windows)

---

### 1. TỔNG QUAN DỰ ÁN (PROJECT OVERVIEW)
Mục tiêu là xây dựng một trợ lý ảo cá nhân (Agent) chạy ngầm trên hệ điều hành Windows. Agent này có nhiệm vụ chủ động giám sát một file Excel checklist công việc cố định, sử dụng mô hình ngôn ngữ lớn (LLM) để phân tích dữ liệu, theo dõi lịch sử nhắc nhở qua cơ sở dữ liệu SQLite cục bộ, và đưa ra thông báo cho người dùng thông qua một giao diện widget (popup/chatbox) PyQt6 dạng overlay không viền hiện đại.

---

### 2. CÔNG NGHỆ & KIẾN TRÚC (TECH STACK & ARCHITECTURE)
* **Ngôn ngữ phát triển:** Python 3.10+
* **Hệ điều hành mục tiêu:** Windows
* **Khung điều khiển Agent (Core Framework):** LangGraph (Hệ sinh thái LangChain) nhằm quản lý trạng thái tư duy và luồng lặp (Stateful Multi-step Agent).
* **Xử lý dữ liệu Excel:** Pandas & Openpyxl.
* **Giám sát tệp tin hệ thống:** Watchdog (Cơ chế kích hoạt dựa trên sự kiện thay đổi file).
* **Bộ nhớ dài hạn (Long-term Memory):** SQLite (Lưu trữ lịch sử nhắc việc, tránh trùng lặp và cấu hình hệ thống).
* **Giao diện người dùng (GUI):** PyQt6 (Thiết kế dạng Frameless Window, bo tròn góc, có độ trong suốt cao làm widget ghim trên màn hình).
* **Kết nối mô hình AI (LLM Connection):** Kiến trúc cắm rút (Pluggable Factory Pattern). Hỗ trợ chuyển đổi linh hoạt giữa Cloud API (OpenAI/Gemini) và Local LLM (Ollama) chạy trên một máy tính khác trong cùng mạng LAN ảo (Virtual LAN).

---

### 3. CẤU TRÚC THƯ MỤC DỰ ÁN (PROJECT DIRECTORY STRUCTURE)

```text
Personal_AI_Agent/
│
├── config/                  
│   └── settings.json        # Lưu đường dẫn file Excel, trạng thái bật tắt LLM, IP kết nối...
│
├── core/                    
│   ├── __init__.py
│   ├── agent_brain.py       # Luồng tư duy chính bằng LangGraph và định nghĩa State
│   ├── excel_connector.py   # Module chuyên trách đọc và ghi dữ liệu Excel theo mẫu
│   └── file_watcher.py      # Module Watchdog giám sát tự động sự kiện lưu file Excel
│
├── ui/                      
│   ├── __init__.py
│   ├── main_window.py       # Cửa sổ PyQt6 chính (Giao diện Chatbox/Popup Overlay)
│   └── components/          # Các thành phần widget bổ trợ
│
├── storage/                 
│   └── agent_memory.db      # Cơ sở dữ liệu SQLite lưu lịch sử hoạt động và bộ nhớ Agent
│
├── templates/               
│   └── checklist_template.xlsx # File Excel checklist mẫu gồm 6 cột
│
├── requirements.txt         # Danh sách thư viện Python cần cài đặt
└── main.py                  # Điểm khởi chạy ứng dụng (App Entry Point)

```

---

### 4. ĐẶC TẢ DỮ LIỆU (DATA SPECIFICATIONS)

#### 4.1. Form Mẫu Excel Checklist (Excel Schema)

File Excel được quét bắt buộc phải tuân theo cấu trúc cố định gồm 6 cột sau:

1. **STT** (Integer): Số thứ tự, tự động tăng khi Agent ghi thêm công việc mới.
2. **TÊN CÔNG VIỆC** (String): Nội dung công việc cụ thể. Đây là dữ liệu cốt lõi để LLM phân tích.
3. **TRẠNG THÁI** (String): Chỉ nhận 1 trong 3 giá trị: `"Đang thực hiện"`, `"Chưa hoàn thành"`, hoặc `"Đã hoàn thành"`.
4. **THỜI HẠN** (Date/String): Hạn chót của công việc.
5. **NGÀY TẠO** (Date: YYYY-MM-DD): Ngày công việc được khởi tạo.
6. **GHI CHÚ** (String): Thông tin bổ sung ngữ cảnh cho công việc.

#### 4.2. Cấu Trúc File Cấu Hình (`config/settings.json`)

```json
{
  "llm_toggle": "local_vlan",
  "excel_file_path": "templates/checklist_template.xlsx",
  "network_timeout_seconds": 5,
  "providers": {
    "local_vlan": {
      "base_url": "[http://10.](http://10.)x.x.x:11434",
      "model_name": "qwen2.5-coder:7b"
    },
    "cloud": {
      "base_url": "[https://api.openai.com/v1](https://api.openai.com/v1)",
      "model_name": "gpt-4o-mini"
    }
  }
}

```

*(Trong đó `10.x.x.x` là địa chỉ IP của máy chủ chạy Model trong mạng LAN ảo).*

---

### 5. ĐẶC TẢ TÍNH NĂNG CỐT LÕI (CORE FUNCTIONAL SPECIFICATIONS)

#### 5.1. Xử Lý Tầng Dữ Liệu Excel (Excel Connector)

* **Chế độ Đọc (Read Mode):** Parse toàn bộ file Excel. Lọc bỏ các dòng có trạng thái `"Đã hoàn thành"`. Chỉ giữ lại và đẩy các dòng có trạng thái `"Đang thực hiện"` và `"Chưa hoàn thành"` vào trạng thái hệ thống (State) của LangGraph.
* **Chế độ Ghi (Write Mode):** Nhận câu lệnh bằng ngôn ngữ tự nhiên từ UI Chatbox, LLM trích xuất các trường thông tin phù hợp, tìm dòng trống cuối cùng trong file Excel, điền thông tin vào và đặt trạng thái mặc định của công việc mới là `"Đang thực hiện"`.

#### 5.2. Cơ Chế Kích Hoạt Chủ Động (Proactive Pull)

Agent tự động quét dữ liệu file Excel trong 2 trường hợp:

1. **Khởi động hệ thống (On Startup):** Quét ngay khi ứng dụng vừa được bật để tổng hợp công việc đầu ngày.
2. **Phát hiện thay đổi (On File Change):** Sử dụng `watchdog` chạy ngầm. Khi người dùng thao tác chỉnh sửa thủ công và lưu file Excel, Agent lập tức phát hiện sự thay đổi và tự động kích hoạt tiến trình nạp lại dữ liệu (Re-scan).

#### 5.3. Luồng Xử Lý Của Bộ Não & Bộ Nhớ (LangGraph & SQLite Logic)

* **Phân loại ưu tiên:** LLM sẽ đọc danh sách việc chưa xong, đối chiếu ngày hiện tại với cột `THỜI HẠN` để phân loại: Việc quá hạn (`Chưa hoàn thành`), việc phải làm trong ngày, việc dài hạn.
* **Cơ chế chống làm phiền (De-duplication Memory):** Trước khi bắn thông báo ra màn hình, Agent phải đối chiếu với DB SQLite (`agent_memory.db`). Nếu một công việc có ID cụ thể đã được thông báo thành công trong ngày và không có thay đổi nội dung, Agent sẽ bỏ qua để tránh spam người dùng.
* **Cơ chế phòng vệ mạng (Network Fallback Shield):** Khi gọi API tới máy chủ LLM qua LAN ảo, nếu quá thời gian `network_timeout_seconds` không kết nối được, hệ thống không được crash mà phải bắt ngoại lệ (Exception), ghi log và trả về một thông báo lỗi nhẹ lên GUI cho người dùng biết hệ thống đang mất kết nối với Server.

#### 5.4. Giao Diện Người Dùng (PyQt6 UI Layer)

* **Phong cách thiết kế:** Giao diện widget nhỏ gọn ghim trên màn hình, thiết kế không viền (Frameless), bo góc, hỗ trợ nền trong suốt/mờ (Acrylic effect).
* **Quy tắc luồng (Threading Rule):** Toàn bộ tiến trình xử lý đọc/ghi file Excel và gọi API LLM (qua LAN hoặc Cloud) bắt buộc phải chạy trên một luồng nền tách biệt (`QThread`), không được chạy trên luồng giao diện chính (Main UI Thread) để tránh hiện tượng đơ ứng dụng (GUI Freezing).

---

### 6. LỘ TRÌNH TRIỂN KHAI CHO CODING AGENT (ROADMAP)

* **Bước 1:** Khởi tạo cấu trúc thư mục, thiết lập `requirements.txt` và xây dựng hoàn chỉnh module `core/excel_connector.py` (Đọc/Lọc/Ghi file Excel theo đúng form mẫu). Test độc lập trên Terminal.
* **Bước 2:** Xây dựng cấu trúc đồ thị LangGraph trong `core/agent_brain.py`, thiết lập lớp trừu tượng để chuyển đổi model thông qua file cấu hình JSON và tích hợp DB SQLite để quản lý lịch sử nhắc việc.
* **Bước 3:** Viết module `core/file_watcher.py` bằng watchdog để bắt sự kiện lưu file Excel và kết nối luồng tự động quét.
* **Bước 4:** Xây dựng giao diện PyQt6 trong `ui/main_window.py`, thiết lập cơ chế đa luồng (`QThread`) để kết nối mượt mà giao diện với bộ não xử lý phía dưới. Hoàn thiện file chạy chính `main.py`.

```

```
