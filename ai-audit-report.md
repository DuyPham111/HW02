# AI Audit Report — HW02 Domain Testing

**Sinh viên:** [HỌ TÊN] ([MSSV])

**Declaration:** *"I use AI tools for the following tasks."*

> Quy tắc: ghi 1 block NGAY SAU mỗi phiên dùng AI (đừng dồn về cuối — sẽ quên prompt/giờ).
> Human Review Notes là phần quan trọng nhất: bạn đã kiểm chứng/sửa/loại gì và vì sao.

---

### [LOG-001] — Chuẩn bị: phân tích bài mẫu + thiết kế cấu trúc bài nộp & template
- **Tool:** Claude Code (Fable 5)
- **Date & Time:** 2026-07-08 (buổi tối, trước 22:00)
- **Prompt:** "Bạn là Senior QA Engineer… phân tích cấu trúc bài nộp và các template của [2 bài làm mẫu], thiết kế cấu trúc nộp bài phù hợp nhất cho tôi… lập file guide.md hướng dẫn từng bước… thiết kế mẫu sẵn các file report để tôi điền" (kèm đề bài HW02, roadmap đã chọn, source code eshop-sut, 2 bài mẫu HW02-Software-Testing-main và HW02-QA-main).
- **AI Output:** Bảng so sánh 2 bài mẫu (định dạng, per-feature, bug report, AI audit, tỷ lệ thực thi); đề xuất cấu trúc lai (Markdown, 4 file/feature, bug report dạng block, audit có Human Review Notes); tạo `guide.md`, bộ template ở root, 16 file template feature, 3 agent skills.
- **Human Review Notes:** Sinh viên đọc lại guide + template; xác nhận với giảng viên 2 điểm treo mà AI gợi ý (phạm vi UI-only ✔ và repo GitHub Issues ✔) rồi yêu cầu AI cập nhật lại nội dung theo xác nhận.

### [LOG-002] — Setup môi trường + FR02 Bước 1 (Business Analysis spec-vs-code)
- **Tool:** Claude Code (Fable 5)
- **Date & Time:** 2026-07-08 22:40
- **Prompt:** "thầy tôi vừa xác nhận HW02 là Functional Testing bắt đầu tương tác từ web/mobile UI nên nếu cần hãy sửa lại nội dung nếu làm api… và https://github.com/DuyPham111/HW02/issues (repo cá nhân). Hãy làm bước tiếp theo cho tôi"
- **AI Output:** (1) Sửa guide/template về phạm vi UI-only + điền URL repo. (2) Cài npm cho backend + 3 frontend, seed DB, chạy backend :3000, smoke test GET /api/products = HTTP 200. (3) `git init` + commit skeleton + remote origin. (4) Sửa `API_URL` mobile → IP LAN 192.168.1.12. (5) Điền mục 1 (Business Analysis) của `features/FR02-Login-Lockout/01-domain-testing.md`: bảng spec-vs-code với 6 rule, 6 candidate bug CB-1…CB-6. Phát hiện mới so với roadmap: **cả web lẫn mobile đều nuốt message backend** (`Login.jsx:18`, `App.js:205`) — giả định "web hiển thị message gốc" trong roadmap là sai; guide phần Feature D đã được sửa theo.
- **Human Review Notes:** [SINH VIÊN ĐIỀN SAU KHI KIỂM CHỨNG: chạy checklist kiểm chứng bằng tay ở mục 1.3 của file 01-domain-testing.md — đếm số lần sai đến khi khóa, đo thời gian khóa, kiểm tra type ô email/mật khẩu, đối chiếu DevTools — rồi ghi kết quả và những gì phải sửa so với phân tích của AI.]

### [LOG-003] — [...]
- **Tool:**
- **Date & Time:**
- **Prompt:**
- **AI Output:**
- **Human Review Notes:**

<!-- Copy block để thêm. Đánh số liên tục LOG-003, LOG-004...
     Mỗi feature thường có 4–6 log (BA, domain TC, BVA, bug report wording, gap analysis). -->
