# Phân tích Nguyên nhân Gốc rễ & Bài học Sửa đổi (5-Whys)

## 📊 1. Tổng quan kết quả kiểm thử

| Phiên bản | Số kịch bản pass | Tỉ lệ | Kịch bản fail |
|-----------|------------------|-------|---------------|
| **V1**    | 2/10             | 20%   | #2, 3, 4, 5, 6, 7, 9, 10 |
| **V2**    | 9/10             | 90%   | #10 (font toán học) |

---

## 🔍 2. Phân tích 5-Whys

### Câu hỏi 1: Tại sao xảy ra các lỗi trên?
**Trả lời:** Do prompt còn nhiều lỗ hổng semantic, chưa thật sự chặt chẽ.

### Câu hỏi 2: Tại sao prompt chưa chặt chẽ?
**Trả lời:** Do prompt chưa chỉ dẫn chính xác cũng như giới hạn đầy đủ những việc AI cần làm. Người thiết kế prompt chưa tính đến phần thiếu tương thích của mã VBA cho tiếng Việt.

### Câu hỏi 3: Tại sao prompt chưa giới hạn đầy đủ?
**Trả lời:** Do prompt chưa giới hạn phạm vi công việc AI chỉ trong việc thiết kế slide, không được thực hiện các công việc khác ngoài yêu cầu.

### Câu hỏi 4: Tại sao prompt chưa chỉ dẫn chính xác?
**Trả lời:** Do prompt chưa đưa ra hướng ứng xử chuẩn mực cho AI khi gặp các trường hợp mơ hồ, dữ liệu mâu thuẫn hay các trường hợp sensitive/edge khác.

### Câu hỏi 5: Tại sao người thiết kế prompt chưa tính đến phần thiếu tương thích của mã VBA cho tiếng Việt?
**Trả lời:** Do thiếu kinh nghiệm trong việc sử dụng các phần mềm Office của Microsoft nên không biết nhược điểm của VBA là không hỗ trợ UTF-8.

---

## 🛠️ 3. Bài học sửa đổi (Lessons Learned)

### 3.1. Về Prompt Engineering

| Vấn đề V1 | Giải pháp V2 |
|-----------|---------------|
| Prompt lỏng lẻo, thiếu guardrails | Thêm **PHẦN 1: GUARDRAILS** với 5 nhóm quy tắc rõ ràng |
| Không yêu cầu Human Checkpoint | Thêm **PHẦN 3: QUY TRÌNH TƯƠNG TÁC** bắt buộc 3 bước |
| Không xử lý edge cases | Thêm quy tắc cụ thể cho từng loại edge case |
| Không giới hạn thẩm quyền | Thêm mục "Vượt thẩm quyền" vào Guardrails |

### 3.2. Về Công nghệ

| Vấn đề V1 | Giải pháp V2 |
|-----------|---------------|
| VBA không hỗ trợ UTF-8 → vỡ font tiếng Việt | Chuyển sang **Python + python-pptx** (UTF-8 native) |
| Không kiểm soát được định dạng | Dùng thư viện `python-pptx` với API rõ ràng |

### 3.3. Về Quy trình

| Vấn đề V1 | Giải pháp V2 |
|-----------|---------------|
| AI tự tạo slide trước khi duyệt | Bắt buộc **Human Checkpoint** ở Bước 2 |
| Không hỏi lại khi mơ hồ | Thêm quy tắc "HỎI LẠI XÁC NHẬN" |
| Không cảnh báo dữ liệu phi logic | Thêm quy tắc "DỪNG LẠI, LIỆT KÊ ĐIỂM BẤT THƯỜNG" |

---

## 🧪 4. Nhật ký thử nghiệm đổi mô hình độc lập (Model Swap Test)

| Tiêu chí | Gemini AI | DeepSeek AI |
|----------|-----------|-------------|
| Đọc prompt V2 | ✅ Tốt | ✅ Tốt |
| Tạo slide đầy đủ | ✅ | ✅ |
| Chủ động gợi ý cải thiện nội dung | ❌ | ✅ (không vượt quyền, có thông báo) |
| Tuân thủ Guardrails | ✅ | ✅ |

**Kết luận:** Cả hai mô hình đều thực hiện tốt yêu cầu tạo slide ở mức độ đầy đủ. DeepSeek AI có ưu điểm chủ động đưa ra lời khuyên cải thiện nội dung bài báo cáo mà không tự ý thêm vào — điều này thể hiện khả năng cân bằng giữa **hỗ trợ** và **tuân thủ guardrails**.

---

## 📌 5. Kết luận

- **V1 → V2:** Tỉ lệ pass tăng từ **20% → 90%**.
- **Nguyên nhân chính:** Prompt V1 thiếu guardrails, thiếu quy trình tương tác, và dùng công nghệ VBA lỗi thời.
- **Bài học lớn nhất:** Prompt cần được thiết kế như một **hợp đồng chặt chẽ** giữa AI và con người, với các điều khoản rõ ràng về:
  - Phạm vi công việc.
  - Cách xử lý edge cases.
  - Điểm dừng bắt buộc (Human Checkpoint).
- **Hạn chế còn lại (V2):** Hiển thị ký hiệu toán học chưa như ý → cần nghiên cứu thêm về font và LaTeX rendering trong python-pptx.