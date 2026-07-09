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

### [LOG-005] — Sinh sẵn test case cụ thể cho cả 4 feature + nâng cấp Bug_Report
- **Tool:** Claude Code (Fable 5)
- **Date & Time:** 2026-07-09
- **Prompt:** "Hãy giúp tôi tham khảo bài làm mới này [SoftwareTesting_HW02] … chắt lọc tinh túy … kết hợp các bài làm trước đó (có image) thiết kế lại … thầy tôi cho phép hỗ trợ sinh testcase nên hãy chỉnh sửa lại các prompt để … sinh sẵn testcase cho tôi test và chụp ảnh bỏ vào"
- **AI Output:** (1) Phân tích bài mẫu mới (format bug table giàu thông tin Pre/Steps/Expected/Actual/Env/Severity, domain 4 bước gọn, nhưng THIẾU ảnh). (2) Sinh sẵn test case cụ thể (giá trị input + thao tác UI + Expected theo spec, Actual để trống) cho FR-09 (9 DT + 7 BVA + 3 robust), FR-15 (8 DT + 5 BVA + 2 robust), FR-02 Mobile (5 DT + 4 BVA). (3) Kiểm tra admin UI: phát hiện admin chặn user thường đăng nhập (`App.jsx:65`) → bug access control FR-15 (R5) chỉ chạm được từ API, **loại khỏi TC UI**, chuyển sang Gap Analysis. (4) Nâng cấp `Bug_Report.md` sang bảng Defect giàu thông tin NHƯNG giữ cột Screenshot + GitHub Issue. (5) Sinh chuỗi 255/256 ký tự sẵn (`reports/test-data-helpers.md`). (6) Cập nhật 2 skill cho phép sinh test case cụ thể; cập nhật guide (bước 1–3 AI đã điền, bạn tập trung bước 4–6).
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã đọc lại toàn bộ test case sinh sẵn; xác nhận Expected khớp spec; chưa chạy nên chưa có Actual. Lưu ý: mọi cột "dự đoán" trong báo cáo là suy luận từ code, KHÔNG được chép thành Actual — phải tự chạy trên UI.]

### [LOG-006] — Thực thi FR-02 trên UI thật (Domain + BVA), xác nhận 3 bug
- **Tool:** Claude Code (Sonnet 5), hướng dẫn từng bước trực tiếp trong lúc sinh viên thao tác trên trình duyệt
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên yêu cầu hướng dẫn chạy từng test case, gửi ảnh chụp màn hình + tab DevTools Network sau mỗi thao tác thật (đăng nhập sai liên tục, đăng nhập đúng khi đang khóa, đo thời gian khóa ở nhiều mốc).
- **AI Output:** Dẫn dắt sinh viên qua 3 đợt test trên UI thật: (1) quan sát nhanh — xác nhận B004 (mật khẩu hiện rõ), B005 (email không validate định dạng), phát hiện thêm B012 (tiêu đề trang sai "Đăng Ký"); (2) chuỗi đăng nhập sai — xác nhận qua Network tab: khóa (HTTP 403) xuất hiện ngay lần sai thứ 3, sớm hơn 1 lần so với thiết kế đúng (B001), kèm phân tích kỹ thứ tự xử lý request để giải thích chính xác vì sao; (3) đo thời gian khóa — sau khi sinh viên tự sửa lại số liệu ban đầu bị nhầm ("~30s"), đo lại chính xác bằng đồng hồ tay ở các mốc 30s/1p/2p/3p, xác nhận khóa kéo dài **hơn 3 phút**, khớp chính xác hằng số `180000` trong code (B002); cũng xác nhận B003 (UI không phân biệt sai mật khẩu vs bị khóa) qua bằng chứng HTTP 403 + thông báo chung. Điền Actual/Pass-Fail cho 9/9 TC Domain đã thiết kế và 7/7 TC BVA chính của FR-02; bổ sung 3 dòng học được vào AI Gap Analysis (lỗi dự đoán ban đầu về thời điểm chính xác xuất hiện 403, thiết kế BVA quá sát biên trước khi biết độ lệch bug lớn, rủi ro chèn test case khác làm hỏng phép đo thời gian).
- **Human Review Notes:** Sinh viên là người trực tiếp thao tác, chụp ảnh, đọc số liệu thật từ DevTools và **tự phát hiện + sửa lại** một số liệu ban đầu báo nhầm (ước lượng "~30 giây" ban đầu, sau đo lại chính xác là hơn 3 phút) — đúng tinh thần không tin tưởng mù quáng vào ước lượng chưa đo kỹ. Ảnh bằng chứng đã lưu vào `reports/FR-02_bugs/` (B001–B005, B012).

### [LOG-007] — Hoàn thiện FR-02: gán TC-ID cho 2 bug quan sát, nhúng ảnh, chuẩn bị GitHub Issues
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên xác nhận đã lưu đủ 6 ảnh (B001–B005, B012) vào thư mục; gửi thêm ảnh xác nhận DT-05/DT-08 Pass; yêu cầu hoàn thiện toàn bộ FR-02 ở cả Main_Report và Bug_Report trước khi chuyển sang FR-09.
- **AI Output:** (1) Điền Actual cho DT-08 (Pass, 401→200 xác nhận reset bộ đếm). (2) Gán TC-ID chính thức FR02-DT-09/DT-10 cho 2 phát hiện B004/B012 (trước đó chỉ nằm trong bảng "quan sát bổ sung", không có TC-ID) để coverage đầy đủ. (3) Cập nhật Tóm tắt coverage Domain (11 EP / 10 TC / 0 chưa cover) và Metrics FR-02 (21 designed, 17 executed, 9 pass, 8 fail, 4 not-run, 6 bugs). (4) Thêm bảng tóm tắt "Confirmed Failed Test Cases" liên kết TC-ID ↔ Bug-ID ↔ screenshot. (5) Bug_Report: sửa đường dẫn B002 (2 file thật: B002-start.png, B002-endafter3minutes.png), thêm mục nhúng ảnh trực tiếp cho 6 bug FR-02 để hiện khi xem Markdown/xuất PDF. (6) Nêu rõ giới hạn: AI không có quyền truy cập GitHub của sinh viên nên **không tự tạo được GitHub Issues** — việc này sinh viên phải tự làm thủ công.
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã tạo xong 6 GitHub Issues cho FR-02 chưa? Đã dán đủ 6 link vào cột "GitHub Issue" trong Bug_Report.md chưa?]

### [LOG-008] — Chạy nốt 4 test case Robust, FR-02 hoàn tất 100%
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên chọn chạy luôn 4 test case Robust (BV-R01..R04) trước khi chuyển feature, gửi kết quả: R01 popup HTML5 chặn, R02/R03/R04 đều HTTP 401.
- **AI Output:** Điền Actual/Pass-Fail cho cả 4 case: R01 Pass (browser chặn đúng); R02 Pass (server không trim khoảng trắng nhưng spec không yêu cầu nên không tính là bug, chỉ ghi chú UX); R03 Pass (SQL injection bị từ chối an toàn, xác nhận SEC-05 — parameterized query hoạt động đúng); R04 Pass (input 500 ký tự không gây crash). Cập nhật Metrics FR-02 cuối cùng: 21 designed / 21 executed / 13 pass / 8 fail / 0 not-run / 6 bugs.
- **Human Review Notes:** [SINH VIÊN ĐIỀN: xác nhận đã đọc lại toàn bộ FR-02 trong Main_Report + Bug_Report, không còn ô trống nào; đã tạo GitHub Issues chưa (xem LOG-007).]

### [LOG-009] — FR-09 Vòng 1: 6 TC Domain đầu + phát hiện B007 bằng số liệu thật + 2 bug ngoài phạm vi
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên báo kết quả 6 test (DT-01..06) kèm 6 ảnh chụp; nêu thêm 2 quan sát ngoài luồng: ô tổng tiền có thể chỉnh sửa, giỏ hàng không xóa sau thanh toán.
- **AI Output:** (1) Xác nhận B007 (công thức percent sai) bằng dữ liệu thật: giỏ 350.000 + SAVE10 → "Tiết kiệm -3.150.000 ₫", "Thành tiền 3.500.000 ₫" — khớp chính xác phép tính `total*(1-10)` từ code. (2) DT-02/03/04/06 đều Pass, khớp spec. (3) Đọc code `Checkout.jsx` xác nhận DT-05 KHÔNG phải bug: nút "Áp dụng" có `disabled` khi input rỗng nên không bấm được (client chặn đúng cách) — sửa lại Expected/Actual cho phù hợp thay vì báo Fail sai. (4) Xác nhận qua code 2 phát hiện ngoài luồng của sinh viên là bug THẬT: ô `editableTotal` là `<input type="number">` không `readOnly`, gửi thẳng lên `/api/checkout`; và `clearCart` được import nhưng không bao giờ được gọi trong `handleCheckout`. Thêm B013, B014 (Feature: FR-08, ngoài 4 feature chấm điểm nhưng vẫn báo cáo theo yêu cầu đề "report all discovered bugs").
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã lưu ảnh hình 1 (bằng chứng B007) vào `FR-09_bugs/B007.png` chưa? Đã làm bước xác nhận bổ sung cho B013 (sửa tổng tiền rồi thanh toán, kiểm tra lịch sử đơn hàng xem server có lưu giá trị giả không) chưa?]

### [LOG-010] — Xác nhận B013 bằng đơn hàng thật, gắn lại đúng feature theo ràng buộc nhóm
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi ảnh Lịch sử đơn hàng (đơn #3, Tổng tiền 1.000 ₫) xác nhận backend lưu đúng giá trị đã sửa tay; đồng thời nêu ràng buộc của giảng viên (không được trùng FR đã chọn với thành viên nhóm khác) và yêu cầu đổi nhãn feature của B013 từ FR-08 sang FR-09 (vì liên quan phần thanh toán), hoặc bỏ nếu không cần thiết.
- **AI Output:** Cập nhật B013: đổi Feature từ "FR-08 (ngoài phạm vi)" sang **FR-09**, giải thích căn cứ hợp lý (ô `editableTotal` chính là `total_amount` được dùng chung cho cả API coupon và API checkout, nên không phải phát hiện ngoài luồng mà thuộc luồng dữ liệu FR-09); điền Actual với bằng chứng đơn hàng thật (#3, 1.000 ₫). Loại bỏ B014 (giỏ hàng không xóa) khỏi Bug_Report chính thức vì không có liên hệ trực tiếp với FR-09, để tránh rủi ro gán nhầm FR ngoài phạm vi 4 feature đã chọn — xóa luôn thư mục `FR-08_bugs` không dùng nữa.
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã lưu ảnh Lịch sử đơn hàng vào `FR-09_bugs/B013.png` chưa? Đồng ý với quyết định bỏ B014 hay muốn giữ lại dưới hình thức khác (vd ghi chú phụ, không tính điểm)?]

### [LOG-011] — DT-07/08/09 xác nhận thêm B008, B007 (số lớn), và demo B013+B007 cộng hưởng
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi 5 ảnh: (1-2) demo mới cho B013 — sửa tổng tiền thành 35.000.000 rồi áp SAVE10, xác nhận đơn hàng thật lưu 350.000.000; (3) DT-07 kết quả; (4) DT-08 kết quả; (5) DT-09 kết quả.
- **AI Output:** Viết lại B013 theo demo mạnh hơn (35tr → áp SAVE10 → 350tr, xác nhận qua đơn #4) cho thấy 2 bug cộng hưởng. Xác nhận B008 bằng dữ liệu thật: khách chưa đăng nhập áp BIGBUY thành công (vi phạm C4), nhưng bước thanh toán cuối cùng vẫn bị chặn đúng do endpoint khác có `authenticateToken` — ghi rõ ranh giới giữa 2 endpoint để không quy kết nhầm toàn bộ luồng là hổng bảo mật. Xác nhận B007 lần 2 với số tiền lớn (30 triệu → -270tr/300tr). DT-09 Pass (VIP100 đúng chặn ở 2/2 lượt) — dùng chung làm bằng chứng cho BV-07; BV-06 (usage=1) vẫn chưa có test riêng.
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã lưu 5 ảnh vào `FR-09_bugs/` với tên đúng (B013.png ghi đè bản cũ, B008.png, và ảnh DT-08 nếu muốn thêm bằng chứng) chưa?]

### [LOG-012] — Xác nhận B006 (off-by-one biên min_order_amount) bằng dữ liệu thật
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi 5 ảnh kết quả BV-01..05 (biên SAVE10 300.000 và BIGBUY 500.000).
- **AI Output:** Xác nhận B006 chính xác: giỏ 299.999 và giỏ ĐÚNG 300.000 (on-point) cho **cùng kết quả từ chối** — chứng minh rõ ràng bug `>` thay vì `>=`. BV-03 (300.001) cho thấy 2 tầng: quyết định accept/reject đúng, nhưng số tiền vẫn sai do B007 tái xuất hiện (áp dụng cho mọi mức tổng tiền, không riêng số lớn). Phát hiện sinh viên đã lỡ dùng nhầm mã `VIP100` thay vì `BIGBUY` khi test BV-05 (500.000) — kết quả nhận được (giảm 100.000 cố định) không phản ánh đúng biên BIGBUY cần test, nên đánh dấu "Not run", yêu cầu làm lại đúng mã.
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã lưu ảnh 2 (BV-02, evidence B006) vào `FR-09_bugs/B006.png` chưa?]

### [LOG-013] — BV-06 + phát hiện DB đã reset giữa các vòng test
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi ảnh BV-06 (VIP100 chấp nhận, giảm 100.000). Yêu cầu tổng hợp hoàn thiện FR-09, liệt kê phần còn thiếu và tên/vị trí ảnh cần lưu.
- **AI Output:** Trước khi ghi nhận, truy vấn trực tiếp DB phát hiện `coupon_usage` và `orders` đã trống, sản phẩm mồi cũ (TEST-350k, TEST-299999...) đã mất — xác nhận đã có 1 lần `node database.js` chạy lại ngoài ý muốn giữa các vòng test trước đó, làm mất trạng thái usage đã tích lũy. Ghi nhận BV-06 là Pass nhưng chú thích rõ đây là lượt dùng ĐẦU TIÊN (0→1) do reset, không phải đúng điểm biên (1→2) như thiết kế ban đầu — vẫn hợp lệ làm bằng chứng "chấp nhận khi còn lượt" nhưng kém sát biên hơn dự tính.
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã lưu ảnh BV-06 vào đúng tên file chưa — xem bảng tổng hợp bên dưới.]

### [LOG-014] — BV-05 làm lại đúng + 3 robust, FR-09 hoàn tất 100%
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi ảnh BV-05 làm lại đúng mã BIGBUY (bị từ chối ở 500.000) và kết quả 3 test robust (R01 disabled, R02 tự uppercase, R03 tự trim).
- **AI Output:** Xác nhận B006 là lỗi HỆ THỐNG (cùng dòng code `>` áp dụng cho mọi coupon) — không chỉ SAVE10 mà cả BIGBUY đều bị từ chối sai ở đúng ngưỡng, làm bằng chứng mạnh hơn nhiều so với chỉ 1 coupon. 3 robust đều Pass đúng như dự đoán từ việc đọc code (client tự uppercase/trim/disable trước khi gửi request) — không phát hiện bug mới nhưng xác nhận dự đoán đúng. Cập nhật Metrics FR-09 cuối: 20/20 executed, 13 pass, 7 fail, 0 not-run, 4 bugs (B006, B007, B008, B013).
- **Human Review Notes:** [SINH VIÊN ĐIỀN: đã tạo GitHub Issues cho 4 bug FR-09 (B006, B007, B008, B013) chưa? Đã lưu đủ ảnh B006.png và B008.png chưa (2 ảnh còn thiếu từ vòng trước)?]

<!-- Copy block để thêm. Đánh số liên tục LOG-015, LOG-016...
     Mỗi feature thường có 4–6 log (BA, domain TC, BVA, bug report wording, gap analysis). -->
