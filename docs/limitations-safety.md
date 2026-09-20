# Khuyến cáo An toàn, Rủi ro Cố hữu & Ranh giới Sử dụng

## ⚠️ 1. Rủi ro cố hữu (Inherent Risks)

### 1.1. Hallucination — Bịa đặt thông tin
- **Mô tả:** AI có thể tự thêm số liệu, tên riêng, thuật ngữ không có trong tài liệu gốc.
- **Hậu quả:** Slide chứa thông tin sai lệch → ảnh hưởng uy tín học thuật.
- **Giảm thiểu:** Guardrail "No Hallucination" + Human Checkpoint bắt buộc.

### 1.2. Prompt Injection
- **Mô tả:** Kẻ tấn công chèn lệnh độc hại trong PDF/Word (dạng chữ ẩn).
- **Hậu quả:** AI thực thi lệnh ngoài ý muốn (VD: tạo slide "Chicken").
- **Giảm thiểu:** Guardrail phớt lờ lệnh độc hại + kiểm tra mã sinh ra.

### 1.3. Lỗi font tiếng Việt
- **Mô tả:** VBA không hỗ trợ UTF-8 → chữ tiếng Việt bị lỗi ("Bối cảnh" → "B?i c?nh").
- **Hậu quả:** Slide không đọc được.
- **Giảm thiểu:** Chuyển sang Python `python-pptx` (hỗ trợ UTF-8 native).

### 1.4. Context Overflow
- **Mô tả:** Tài liệu > 25 trang / 10.000 chữ → AI xử lý kém chính xác.
- **Giảm thiểu:** Từ chối và yêu cầu chia nhỏ tài liệu.

### 1.5. Dữ liệu phi logic / mâu thuẫn
- **Mô tả:** Tài liệu chứa số liệu vô lý ("tỉ lệ nam:nữ = 1:1000") hoặc mâu thuẫn.
- **Giảm thiểu:** AI dừng lại, liệt kê điểm bất thường, hỏi người dùng.

## 🚫 2. Ranh giới sử dụng (Out of Scope)

AI **KHÔNG** được phép:

| Hành vi | Lý do từ chối |
|---------|---------------|
| Viết thơ, bài hát từ báo cáo | Vượt thẩm quyền |
| Tư vấn y tế / pháp lý | Rủi ro cao, cần chuyên gia |
| Dự đoán điểm bảo vệ luận văn | Không có cơ sở khoa học |
| Chấm điểm nội dung | Vượt thẩm quyền |
| Truy cập dữ liệu ngoài tài liệu | Vi phạm quyền riêng tư |
| Chỉnh sửa file gốc | Vi phạm tính toàn vẹn |

## ✅ 3. Khuyến cáo sử dụng an toàn

1. **Luôn kiểm duyệt dàn ý** trước khi cho AI sinh mã.
2. **Kiểm tra mã sinh ra** trước khi chạy (đặc biệt nếu tài liệu từ nguồn không tin cậy).
3. **Đối chiếu slide với tài liệu gốc** để phát hiện hallucination.
4. **Không upload tài liệu mật** — AI chỉ nên dùng cho tài liệu công khai.
5. **Giới hạn dung lượng** ≤ 25 trang để đảm bảo chất lượng.

## 🛡️ 4. Cơ chế bảo vệ (Defense in Depth)
Layer 1: System Prompt Guardrails (từ chối sớm)
Layer 2: Human Checkpoint (duyệt dàn ý)
Layer 3: Mã sinh ra có thể audit (Python minh bạch)
Layer 4: Người dùng kiểm tra file .pptx cuối cùng


## 📌 5. Tuyên bố miễn trách

> Hệ thống này là **công cụ hỗ trợ**, không thay thế hoàn toàn con người. Người dùng chịu trách nhiệm cuối cùng về tính chính xác và tính chuyên nghiệp của bài thuyết trình.