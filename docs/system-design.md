
---

# Bản thiết kế hệ thống 11 thành phần (AI System Design Checklist)

## 1. Problem — Vấn đề

### Điểm nghẽn (Bottleneck) trong quy trình thủ công

**A. Chắt lọc nội dung:**
- Phải đọc tài liệu dài (bài báo khoa học có thể 20–25 trang).
- Phải xác định đâu là thông tin quan trọng.
- Phải quyết định nội dung nào nên đưa lên slide.

**B. Thiết kế & phân bổ nội dung:**
- Chọn template phù hợp.
- Chọn bố cục (layout) cho từng slide.
- Phân bổ nội dung vào từng slide.
- Căn chỉnh chữ, vị trí, kích thước.
- Đảm bảo tính đồng nhất giữa các slide.

## 2. User — Người dùng

- **Đối tượng:** Sinh viên, nghiên cứu sinh.
- **Năng lực:** Dàn trải từ người chỉ biết upload file lên giao diện → đến người có kiến thức lập trình chuyên sâu.
- **Kỳ vọng:** Nhận file `.pptx` hoàn chỉnh, có thể chỉnh sửa lại.

## 3. Input — Đầu vào

- **Định dạng chấp nhận:** PDF, Word (.docx), PowerPoint (.pptx).
- **Giới hạn:** ≤ 25 trang / ~10.000 chữ.
- **Xử lý ngoại lệ:**
  - File rỗng → từ chối.
  - File ảnh không có chữ → từ chối.
  - File tiểu thuyết (không phải báo cáo khoa học) → từ chối.

## 4. Context / Data — Ngữ cảnh dữ liệu

**Được phép truy cập:**
- Toàn bộ nội dung bên trong tài liệu được gửi.
- Toàn bộ file trong thư mục gốc chứa tài liệu đó.

**CẤM truy cập:**
- Nội dung không thuộc tài liệu được gửi.
- Dữ liệu bên ngoài thư mục được cấp phép.

## 5. AI Responsibility — Trách nhiệm của AI

1. Đọc toàn bộ nội dung báo cáo.
2. Trích xuất 5–7 ý chính/slide (bullet points).
3. Chọn template phù hợp với nội dung.
4. Xây dựng bố cục hợp lý, đưa nội dung đã chắt lọc vào slide.
5. Sinh mã Python (python-pptx) để tạo file `.pptx`.

## 6. Human Responsibility — Trách nhiệm của con người

- ✅ Kiểm duyệt template & thiết kế tổng thể (đồng nhất, chuyên nghiệp).
- ✅ Kiểm duyệt nội dung từng slide (đúng với bài báo gốc, không bịa).
- ✅ Chỉnh sửa phần còn khuyết.
- ✅ Theo dõi hành vi AI (không vượt quyền truy cập dữ liệu).
- ✅ Xác nhận hoàn thành trước khi cho phép AI xuất mã.
- ✅ Kiểm tra mã sinh ra có bị prompt injection không.

## 7. Workflow — Quy trình 3 chặng

### Chặng 1 — Nhận & Xử lý thô
AI đọc File
→ Kiểm tra đầu vào (file rỗng/lỗi → hỏi lại)
→ Xác định ý chính
→ Xuất "Bản Dàn ý các Slide" (Outline)

### Chặng 2 — Human Checkpoint
Người dùng đọc Bản Dàn ý
→ Chỉnh sửa, thêm/bớt ý
→ Duyệt nội dung


### Chặng 3 — Sinh thành phẩm
AI nhận lệnh chốt
→ Chuyển dàn ý thành mã Python (python-pptx)
→ Người dùng chạy mã → Hoàn thiện file .pptx


## 8. Tools / Integrations

| Thành phần | Công cụ |
|------------|---------|
| Đọc tài liệu | LLM (Gemini / DeepSeek) |
| Sinh mã | Python + `python-pptx` |
| Font | Unicode UTF-8 (hỗ trợ tiếng Việt) |
| Nền tảng | Web app / Chat interface |

## 9. Permissions — Quyền hạn

- **Read-only** cho file đầu vào và dữ liệu trong thư mục được cấp phép.
- **Không có quyền** chỉnh sửa file gốc.
- **Không có quyền** truy cập dữ liệu ngoài phạm vi.

## 10. Guardrails — Rào chắn

**Cấm tuyệt đối:**
- ❌ Truy cập dữ liệu bên ngoài thư mục được cấp phép.
- ❌ Chỉnh sửa nội dung file input.
- ❌ Nhét mã độc vào mã sinh ra.
- ❌ Tự bịa số liệu, tên riêng, thuật ngữ.
- ❌ Vượt thẩm quyền (làm thơ, viết nhạc, tư vấn y tế/pháp lý).
- ❌ Tự tạo slide trước khi người dùng duyệt dàn ý.

## 11. Evaluation — Đánh giá

| # | Tiêu chí | Mô tả | Ngưỡng Pass |
|---|----------|-------|-------------|
| 1 | **Execution** | Mã sinh ra chạy thành công ngay lần đầu | Không lỗi cú pháp/thư viện |
| 2 | **Formatting** | Tuân thủ giới hạn (5–7 bullet/slide, ≤ 20 chữ/dòng) | 100% tuân thủ |
| 3 | **No Hallucination** | 100% số liệu, tên riêng, thuật ngữ có trong tài liệu gốc | 0% bịa đặt |
| 4 | **Edge Cases** | Từ chối đúng khi file rỗng/ảnh/tiểu thuyết | Thông báo từ chối rõ ràng |

---