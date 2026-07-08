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
- **AI Output:** (1) Sửa guide/template về phạm vi UI-only + điền URL repo. (2) Cài npm cho backend + 3 frontend, seed DB, chạy backend :3000, smoke test GET /api/products = HTTP 200. (3) `git init` + commit skeleton + remote origin. (4) Sửa `API_URL` mobile → IP LAN 192.168.1.12. (5) Điền Bước 1 (Business Analysis) chương FEATURE A trong `reports/Main_Report.md`: bảng spec-vs-code 6 rule + candidate bugs. Phát hiện mới so với roadmap: **cả web lẫn mobile đều nuốt message backend** (`Login.jsx:18`, `App.js:205`) — giả định "web hiển thị message gốc" trong roadmap là sai; guide phần Feature D đã được sửa theo.
- **Human Review Notes:** [SINH VIÊN ĐIỀN SAU KHI KIỂM CHỨNG: chạy checklist kiểm chứng bằng tay cuối Bước 1 chương FEATURE A — đếm số lần sai đến khi khóa, đo thời gian khóa, kiểm tra type ô email/mật khẩu, đối chiếu DevTools — rồi ghi kết quả và những gì phải sửa so với phân tích của AI.]

### [LOG-003] — Tái cấu trúc báo cáo theo format chuẩn lớp (6-bước Domain / 4-bước BVA)
- **Tool:** Claude Code (Fable 5)
- **Date & Time:** 2026-07-08 ~23:00
- **Prompt:** "Hãy đọc cách làm bài của 2 bài làm này [23127195, 23127259] cách viết domain… tham khảo sửa lại các phần nội dung giúp tôi và hướng dẫn tôi làm tiếp"
- **AI Output:** Phân tích 2 bài cùng lớp (cùng bộ format: Domain 6 bước có Input Variables ID / Domains / EP-ID / Constraints / TC có cột truy vết + Actual-Pass/Fail-BugID, BVA 4 bước có B-ID / BVA Points / Robust, metrics per feature, 1 Main_Report hợp nhất, Bug_Report block, skills dạng SKILL.md). Tái cấu trúc bài nộp theo format đó: gộp per-feature files vào `reports/Main_Report.md` (4 chương), viết lại `reports/Bug_Report.md`, chuyển 3 skill sang `skills/*/SKILL.md` có frontmatter, cập nhật guide/README. Nội dung Bước 1 FR-02 đã kiểm chứng code được giữ nguyên khi chuyển.
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã đọc lại cấu trúc mới, xác nhận không copy nguyên văn câu chữ/prompt của 2 bạn — chỉ dùng chung khung trình bày kỹ thuật.]

### [LOG-004] — FR02 Bước 3-6 (Domain) + toàn bộ BVA — AI sinh test case lần đầu
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** "nhưng ở mỗi fr tôi chưa thấy bạn tạo test case cho tôi mà chỉ toàn spec" (sau khi hỏi lại giải thích quy trình) — yêu cầu AI thực sự áp dụng skill `hw02-domain-testing` và `hw02-bva` để sinh test case cụ thể cho FR-02, không chỉ để khung template trống.
- **AI Output:** Domains (4 biến, gồm 2 biến ẩn) → 11 Equivalence Partitions (EP-FR02-01..11) → 6 Constraints (thêm C-FR02-06 mới: trạng thái khóa phải override cả khi mật khẩu đúng) → 8 Domain Test Cases (FR02-DT-01..08, single-fault). BVA: 7 BVA points (on-point tại attempts=3 và tại 30s theo spec) → 7 BVA test cases (FR02-BV-01..07) → 4 robust case (rỗng, whitespace, SQLi, input dài). Toàn bộ cột Expected viết THEO SPEC; cột Actual/Pass-Fail để trống chờ chạy thật. AI có dự đoán trước (ghi rõ là dự đoán, không phải Actual) rằng biên thật sẽ lệch ở attempts=2 và ~180s dựa trên phân tích code đã làm ở LOG-002.
- **Human Review Notes:** [SINH VIÊN ĐIỀN SAU KHI CHẠY THẬT: kiểm tra từng EP có bị trùng nghĩa không, từng TC có Expected rõ ràng không, rồi mới chạy để điền Actual — không được điền Actual bằng dự đoán của AI.]

### [LOG-005] — [...]
- **Tool:**
- **Date & Time:**
- **Prompt:**
- **AI Output:**
- **Human Review Notes:**

<!-- Copy block để thêm. Đánh số liên tục LOG-003, LOG-004...
     Mỗi feature thường có 4–6 log (BA, domain TC, BVA, bug report wording, gap analysis). -->
