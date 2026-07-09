# AI Audit Report — HW02 Domain Testing

**Sinh viên:** [Phạm Vũ Ngọc Duy] ([23127183])

**Declaration:** *"I use AI tools for the following tasks."*

> Quy tắc: ghi 1 block NGAY SAU mỗi phiên dùng AI (đừng dồn về cuối — sẽ quên prompt/giờ).
> Human Review Notes là phần quan trọng nhất: bạn đã kiểm chứng/sửa/loại gì và vì sao.

---

### [LOG-001] — Chuẩn bị: phân tích bài mẫu + thiết kế cấu trúc bài nộp & template
- **Tool:** Claude Code (Fable 5)
- **Date & Time:** 2026-07-08 (buổi tối, trước 22:00)
- **Prompt:** "Bạn là Senior QA Engineer… phân tích cấu trúc bài nộp và các template của [2 bài làm mẫu], thiết kế cấu trúc nộp bài phù hợp nhất cho tôi… lập file guide.md hướng dẫn từng bước… thiết kế mẫu sẵn các file report để tôi điền" (kèm đề bài HW02, roadmap đã chọn, source code eshop-sut)
- **AI Output:** Đề xuất cấu trúc lai (Markdown, 4 file/feature, bug report dạng block, audit có Human Review Notes); tạo `guide.md`, bộ template ở root, 16 file template feature, 3 agent skills.
- **Human Review Notes:** Sinh viên đọc lại guide + template; xác nhận với giảng viên 2 điểm treo mà AI gợi ý (phạm vi UI-only ✔ và repo GitHub Issues ✔) rồi yêu cầu AI cập nhật lại nội dung theo xác nhận.

### [LOG-002] — Setup môi trường + FR02 Bước 1 (Business Analysis spec-vs-code)
- **Tool:** Claude Code (Fable 5)
- **Date & Time:** 2026-07-08 22:40
- **Prompt:** "thầy tôi vừa xác nhận HW02 là Functional Testing bắt đầu tương tác từ web/mobile UI nên nếu cần hãy sửa lại nội dung nếu làm api… và https://github.com/DuyPham111/HW02/issues (repo cá nhân). Hãy làm bước tiếp theo cho tôi"
- **AI Output:** (1) Sửa guide/template về phạm vi UI-only + điền URL repo. (2) Cài npm cho backend + 3 frontend, seed DB, chạy backend :3000, smoke test GET /api/products = HTTP 200. (3) `git init` + commit skeleton + remote origin. (4) Sửa `API_URL` mobile → IP LAN 192.168.1.12. (5) Điền Bước 1 (Business Analysis) chương FEATURE A trong `reports/Main_Report.md`: bảng spec-vs-code 6 rule + candidate bugs. Phát hiện mới so với roadmap: **cả web lẫn mobile đều nuốt message backend** (`Login.jsx:18`, `App.js:205`) — giả định "web hiển thị message gốc" trong roadmap là sai; guide phần Feature D đã được sửa theo.
- **Human Review Notes:** Đã tự chạy checklist kiểm chứng bằng tay trên UI thật (không dừng ở đọc code): đăng nhập sai liên tiếp và dùng DevTools Network đếm — xác nhận khóa (403) xuất hiện đúng ở **lần sai thứ 3**, không phải lần thứ 2 như AI dự đoán chỉ từ đọc tĩnh source; đo thời gian khóa bằng đồng hồ tay ở nhiều mốc — xác nhận ~3 phút chứ không phải 30 giây như spec ghi; xác nhận ô mật khẩu không che ký tự và ô email không validate định dạng, khớp với candidate bug AI nêu. Kết quả chi tiết và ảnh bằng chứng nằm ở LOG-004.


### [LOG-003] — FR02 Bước 3-6 (Domain) + toàn bộ BVA — AI sinh test case lần đầu
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** "nhưng ở mỗi fr tôi chưa thấy bạn tạo test case cho tôi mà chỉ toàn spec" (sau khi hỏi lại giải thích quy trình) — yêu cầu AI thực sự áp dụng skill `hw02-domain-testing` và `hw02-bva` để sinh test case cụ thể cho FR-02, không chỉ để khung template trống.
- **AI Output:** Domains (4 biến, gồm 2 biến ẩn) → 11 Equivalence Partitions (EP-FR02-01..11) → 6 Constraints (thêm C-FR02-06 mới: trạng thái khóa phải override cả khi mật khẩu đúng) → 8 Domain Test Cases (FR02-DT-01..08, single-fault). BVA: 7 BVA points (on-point tại attempts=3 và tại 30s theo spec) → 7 BVA test cases (FR02-BV-01..07) → 4 robust case (rỗng, whitespace, SQLi, input dài). Toàn bộ cột Expected viết THEO SPEC; cột Actual/Pass-Fail để trống chờ chạy thật. AI có dự đoán trước (ghi rõ là dự đoán, không phải Actual) rằng biên thật sẽ lệch ở attempts=2 và ~180s dựa trên phân tích code đã làm ở LOG-002.
- **Human Review Notes:** Đã đọc lại 11 EP trước khi chạy, xác nhận không có EP nào trùng nghĩa và Expected của 8 Domain TC + 7 BVA TC đều bám sát spec (không phải bám theo dự đoán bug của AI). Không sửa Expected ở bước này vì chưa có bằng chứng thật — chỉ mới là giả thuyết AI nêu dựa trên đọc code. Toàn bộ Actual chỉ được điền sau khi chạy thật trên UI ở LOG-004, đúng nguyên tắc không lấy dự đoán của AI làm kết quả.


### [LOG-004] — Thực thi FR-02 trên UI thật (Domain + BVA), xác nhận 3 bug
- **Tool:** Claude Code (Sonnet 5), hướng dẫn từng bước trực tiếp trong lúc sinh viên thao tác trên trình duyệt
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên yêu cầu hướng dẫn chạy từng test case, gửi ảnh chụp màn hình + tab DevTools Network sau mỗi thao tác thật (đăng nhập sai liên tục, đăng nhập đúng khi đang khóa, đo thời gian khóa ở nhiều mốc).
- **AI Output:** Dẫn dắt sinh viên qua 3 đợt test trên UI thật: (1) quan sát nhanh — xác nhận B004 (mật khẩu hiện rõ), B005 (email không validate định dạng), phát hiện thêm B012 (tiêu đề trang sai "Đăng Ký"); (2) chuỗi đăng nhập sai — xác nhận qua Network tab: khóa (HTTP 403) xuất hiện ngay lần sai thứ 3, sớm hơn 1 lần so với thiết kế đúng (B001), kèm phân tích kỹ thứ tự xử lý request để giải thích chính xác vì sao; (3) đo thời gian khóa — sau khi sinh viên tự sửa lại số liệu ban đầu bị nhầm ("~30s"), đo lại chính xác bằng đồng hồ tay ở các mốc 30s/1p/2p/3p, xác nhận khóa kéo dài **hơn 3 phút**, khớp chính xác hằng số `180000` trong code (B002); cũng xác nhận B003 (UI không phân biệt sai mật khẩu vs bị khóa) qua bằng chứng HTTP 403 + thông báo chung. Điền Actual/Pass-Fail cho 9/9 TC Domain đã thiết kế và 7/7 TC BVA chính của FR-02; bổ sung 3 dòng học được vào AI Gap Analysis (lỗi dự đoán ban đầu về thời điểm chính xác xuất hiện 403, thiết kế BVA quá sát biên trước khi biết độ lệch bug lớn, rủi ro chèn test case khác làm hỏng phép đo thời gian).
- **Human Review Notes:** Sinh viên là người trực tiếp thao tác, chụp ảnh, đọc số liệu thật từ DevTools và **tự phát hiện + sửa lại** một số liệu ban đầu báo nhầm (ước lượng "~30 giây" ban đầu, sau đo lại chính xác là hơn 3 phút) — đúng tinh thần không tin tưởng mù quáng vào ước lượng chưa đo kỹ. Ảnh bằng chứng đã lưu vào `reports/FR-02_bugs/` (B001–B005, B012).

### [LOG-005] — Hoàn thiện FR-02: gán TC-ID cho 2 bug quan sát, nhúng ảnh, chuẩn bị GitHub Issues
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên xác nhận đã lưu đủ 6 ảnh (B001–B005, B012) vào thư mục; gửi thêm ảnh xác nhận DT-05/DT-08 Pass; yêu cầu hoàn thiện toàn bộ FR-02 ở cả Main_Report và Bug_Report trước khi chuyển sang FR-09.
- **AI Output:** (1) Điền Actual cho DT-08 (Pass, 401→200 xác nhận reset bộ đếm). (2) Gán TC-ID chính thức FR02-DT-09/DT-10 cho 2 phát hiện B004/B012 (trước đó chỉ nằm trong bảng "quan sát bổ sung", không có TC-ID) để coverage đầy đủ. (3) Cập nhật Tóm tắt coverage Domain (11 EP / 10 TC / 0 chưa cover) và Metrics FR-02 (21 designed, 17 executed, 9 pass, 8 fail, 4 not-run, 6 bugs). (4) Thêm bảng tóm tắt "Confirmed Failed Test Cases" liên kết TC-ID ↔ Bug-ID ↔ screenshot. (5) Bug_Report: sửa đường dẫn B002 (2 file thật: B002-start.png, B002-endafter3minutes.png), thêm mục nhúng ảnh trực tiếp cho 6 bug FR-02 để hiện khi xem Markdown/xuất PDF. (6) Nêu rõ giới hạn: AI không có quyền truy cập GitHub của sinh viên nên **không tự tạo được GitHub Issues** — việc này sinh viên phải tự làm thủ công.
- **Human Review Notes:** Đã tự tạo đủ 6 GitHub Issues (#1–#6) cho B001–B005, B012 trên repo cá nhân github.com/DuyPham111/HW02 và dán link thật vào cột "GitHub Issue" của Bug_Report.md — đây là thao tác thủ công bắt buộc, AI không có quyền tạo Issue thay.

### [LOG-006] — Chạy nốt 4 test case Robust, FR-02 hoàn tất 100%
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên chọn chạy luôn 4 test case Robust (BV-R01..R04) trước khi chuyển feature, gửi kết quả: R01 popup HTML5 chặn, R02/R03/R04 đều HTTP 401.
- **AI Output:** Điền Actual/Pass-Fail cho cả 4 case: R01 Pass (browser chặn đúng); R02 Pass (server không trim khoảng trắng nhưng spec không yêu cầu nên không tính là bug, chỉ ghi chú UX); R03 Pass (SQL injection bị từ chối an toàn, xác nhận SEC-05 — parameterized query hoạt động đúng); R04 Pass (input 500 ký tự không gây crash). Cập nhật Metrics FR-02 cuối cùng: 21 designed / 21 executed / 13 pass / 8 fail / 0 not-run / 6 bugs.
- **Human Review Notes:** Đã đọc lại toàn bộ FR-02 trong Main_Report và Bug_Report, xác nhận không còn ô Actual/Pass-Fail nào bỏ trống; 6 GitHub Issues cho FR-02 đã tạo xong ở LOG-005 trước khi chuyển sang FR-09.

### [LOG-007] — FR-09 Vòng 1: 6 TC Domain đầu + phát hiện B007 bằng số liệu thật + 2 bug ngoài phạm vi
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên báo kết quả 6 test (DT-01..06) kèm 6 ảnh chụp; nêu thêm 2 quan sát ngoài luồng: ô tổng tiền có thể chỉnh sửa, giỏ hàng không xóa sau thanh toán.
- **AI Output:** (1) Xác nhận B007 (công thức percent sai) bằng dữ liệu thật: giỏ 350.000 + SAVE10 → "Tiết kiệm -3.150.000 ₫", "Thành tiền 3.500.000 ₫" — khớp chính xác phép tính `total*(1-10)` từ code. (2) DT-02/03/04/06 đều Pass, khớp spec. (3) Đọc code `Checkout.jsx` xác nhận DT-05 KHÔNG phải bug: nút "Áp dụng" có `disabled` khi input rỗng nên không bấm được (client chặn đúng cách) — sửa lại Expected/Actual cho phù hợp thay vì báo Fail sai. (4) Xác nhận qua code 2 phát hiện ngoài luồng của sinh viên là bug THẬT: ô `editableTotal` là `<input type="number">` không `readOnly`, gửi thẳng lên `/api/checkout`; và `clearCart` được import nhưng không bao giờ được gọi trong `handleCheckout`. Thêm B013, B014 (Feature: FR-08, ngoài 4 feature chấm điểm nhưng vẫn báo cáo theo yêu cầu đề "report all discovered bugs").
- **Human Review Notes:** Đã lưu ảnh bằng chứng B007 vào `FR-09_bugs/B007.png`. Đồng thời tự làm thêm bước xác nhận sâu hơn cho B013 mà AI đề xuất (sửa tổng tiền trên UI rồi hoàn tất thanh toán, kiểm tra Lịch sử đơn hàng xem server có lưu đúng giá trị đã sửa tay hay không) — kết quả xác nhận ở LOG-008 bằng đơn hàng thật, không chỉ dừng ở quan sát ô input có thể sửa.

### [LOG-008] — Xác nhận B013 bằng đơn hàng thật, gắn lại đúng feature theo ràng buộc nhóm
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi ảnh Lịch sử đơn hàng (đơn #3, Tổng tiền 1.000 ₫) xác nhận backend lưu đúng giá trị đã sửa tay; đồng thời nêu ràng buộc của giảng viên (không được trùng FR đã chọn với thành viên nhóm khác) và yêu cầu đổi nhãn feature của B013 từ FR-08 sang FR-09 (vì liên quan phần thanh toán), hoặc bỏ nếu không cần thiết.
- **AI Output:** Cập nhật B013: đổi Feature từ "FR-08 (ngoài phạm vi)" sang **FR-09**, giải thích căn cứ hợp lý (ô `editableTotal` chính là `total_amount` được dùng chung cho cả API coupon và API checkout, nên không phải phát hiện ngoài luồng mà thuộc luồng dữ liệu FR-09); điền Actual với bằng chứng đơn hàng thật (#3, 1.000 ₫). Loại bỏ B014 (giỏ hàng không xóa) khỏi Bug_Report chính thức vì không có liên hệ trực tiếp với FR-09, để tránh rủi ro gán nhầm FR ngoài phạm vi 4 feature đã chọn — xóa luôn thư mục `FR-08_bugs` không dùng nữa.
- **Human Review Notes:** Đã lưu ảnh Lịch sử đơn hàng (đơn #3, 1.000 ₫) làm bằng chứng cho B013. Đồng ý với đề xuất bỏ B014 (giỏ hàng không xóa) khỏi Bug_Report chính thức, vì bug này không có liên hệ trực tiếp với luồng dữ liệu FR-09 — giữ lại sẽ tạo rủi ro bị coi là gán nhầm sang FR-08 (feature ngoài phạm vi 4 feature đã chọn), vi phạm ràng buộc "không trùng FR với thành viên nhóm" mà giảng viên yêu cầu.

### [LOG-009] — DT-07/08/09 xác nhận thêm B008, B007 (số lớn), và demo B013+B007 cộng hưởng
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi 5 ảnh: (1-2) demo mới cho B013 — sửa tổng tiền thành 35.000.000 rồi áp SAVE10, xác nhận đơn hàng thật lưu 350.000.000; (3) DT-07 kết quả; (4) DT-08 kết quả; (5) DT-09 kết quả.
- **AI Output:** Viết lại B013 theo demo mạnh hơn (35tr → áp SAVE10 → 350tr, xác nhận qua đơn #4) cho thấy 2 bug cộng hưởng. Xác nhận B008 bằng dữ liệu thật: khách chưa đăng nhập áp BIGBUY thành công (vi phạm C4), nhưng bước thanh toán cuối cùng vẫn bị chặn đúng do endpoint khác có `authenticateToken` — ghi rõ ranh giới giữa 2 endpoint để không quy kết nhầm toàn bộ luồng là hổng bảo mật. Xác nhận B007 lần 2 với số tiền lớn (30 triệu → -270tr/300tr). DT-09 Pass (VIP100 đúng chặn ở 2/2 lượt) — dùng chung làm bằng chứng cho BV-07; BV-06 (usage=1) vẫn chưa có test riêng.
- **Human Review Notes:** Đã lưu đủ 5 ảnh vào `FR-09_bugs/` — B013.png (ghi đè bản demo 35tr mạnh hơn), B008.png, cùng ảnh DT-07/DT-08/DT-09 làm bằng chứng bổ sung. Đã kiểm tra tên file khớp đúng ID bug trước khi commit để tránh lẫn lộn giữa các đợt test.

### [LOG-010] — Xác nhận B006 (off-by-one biên min_order_amount) bằng dữ liệu thật
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi 5 ảnh kết quả BV-01..05 (biên SAVE10 300.000 và BIGBUY 500.000).
- **AI Output:** Xác nhận B006 chính xác: giỏ 299.999 và giỏ ĐÚNG 300.000 (on-point) cho **cùng kết quả từ chối** — chứng minh rõ ràng bug `>` thay vì `>=`. BV-03 (300.001) cho thấy 2 tầng: quyết định accept/reject đúng, nhưng số tiền vẫn sai do B007 tái xuất hiện (áp dụng cho mọi mức tổng tiền, không riêng số lớn). Phát hiện sinh viên đã lỡ dùng nhầm mã `VIP100` thay vì `BIGBUY` khi test BV-05 (500.000) — kết quả nhận được (giảm 100.000 cố định) không phản ánh đúng biên BIGBUY cần test, nên đánh dấu "Not run", yêu cầu làm lại đúng mã.
- **Human Review Notes:** Đã lưu ảnh BV-02 (evidence B006, đối chiếu 299.999 vs 300.000 cùng bị từ chối) vào `FR-09_bugs/B006.png`. Đã tự nhận ra và báo lại việc dùng nhầm mã VIP100 thay vì BIGBUY khi test BV-05, làm lại đúng mã ở vòng sau (LOG-012) thay vì để AI ghi nhận kết quả sai làm Actual.

### [LOG-011] — BV-06 + phát hiện DB đã reset giữa các vòng test
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi ảnh BV-06 (VIP100 chấp nhận, giảm 100.000). Yêu cầu tổng hợp hoàn thiện FR-09, liệt kê phần còn thiếu và tên/vị trí ảnh cần lưu.
- **AI Output:** Trước khi ghi nhận, truy vấn trực tiếp DB phát hiện `coupon_usage` và `orders` đã trống, sản phẩm mồi cũ (TEST-350k, TEST-299999...) đã mất — xác nhận đã có 1 lần `node database.js` chạy lại ngoài ý muốn giữa các vòng test trước đó, làm mất trạng thái usage đã tích lũy. Ghi nhận BV-06 là Pass nhưng chú thích rõ đây là lượt dùng ĐẦU TIÊN (0→1) do reset, không phải đúng điểm biên (1→2) như thiết kế ban đầu — vẫn hợp lệ làm bằng chứng "chấp nhận khi còn lượt" nhưng kém sát biên hơn dự tính.
- **Human Review Notes:** Đã lưu ảnh BV-06 đúng tên file quy ước. Đồng ý với cách AI xử lý phát hiện DB bị reset ngoài kế hoạch: chấp nhận ghi Pass cho BV-06 nhưng có chú thích rõ đây thực chất là lượt dùng đầu tiên (0→1) chứ không phải đúng điểm biên 1→2 như thiết kế ban đầu, thay vì che giấu giới hạn này của phép đo.

### [LOG-012] — BV-05 làm lại đúng + 3 robust, FR-09 hoàn tất 100%
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi ảnh BV-05 làm lại đúng mã BIGBUY (bị từ chối ở 500.000) và kết quả 3 test robust (R01 disabled, R02 tự uppercase, R03 tự trim).
- **AI Output:** Xác nhận B006 là lỗi HỆ THỐNG (cùng dòng code `>` áp dụng cho mọi coupon) — không chỉ SAVE10 mà cả BIGBUY đều bị từ chối sai ở đúng ngưỡng, làm bằng chứng mạnh hơn nhiều so với chỉ 1 coupon. 3 robust đều Pass đúng như dự đoán từ việc đọc code (client tự uppercase/trim/disable trước khi gửi request) — không phát hiện bug mới nhưng xác nhận dự đoán đúng. Cập nhật Metrics FR-09 cuối: 20/20 executed, 13 pass, 7 fail, 0 not-run, 4 bugs (B006, B007, B008, B013).
- **Human Review Notes:** Đã tạo đủ 4 GitHub Issues (#7–#10) cho B006, B007, B008, B013 và dán link vào Bug_Report.md; đã bổ sung đủ ảnh B006.png và B008.png còn thiếu từ vòng trước. Đọc lại toàn bộ FR-09 trong Main_Report + Bug_Report trước khi chuyển sang FR-15, không còn ô trống.

<!-- Copy block để thêm. Đánh số liên tục LOG-015, LOG-016...
     Mỗi feature thường có 4–6 log (BA, domain TC, BVA, bug report wording, gap analysis). -->

### [LOG-013] — FR-15 DT-01..07 hoàn tất (2 bug mới B009, B010); DT-08 tạm hoãn
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên gửi kết quả DT-01..07 kèm 5 ảnh; xác nhận DT-04 hiển thị nguyên văn `<b>Sony</b>` (an toàn); yêu cầu tạm gác DT-08 để hỏi sau, chốt xong DT-01..07 trước.
- **AI Output:** Xác nhận DT-01, DT-02, DT-04 Pass (đúng spec); DT-03 Fail — dùng lại ID có sẵn **B010** (tên chỉ khoảng trắng vẫn tạo được, không phải bug mới); DT-05/06/07 Fail — dùng lại ID có sẵn **B009**, gộp cả 3 trường hợp (giá 0/âm/rỗng) vào 1 bug vì cùng nguyên nhân gốc (backend không validate `price`, ô price client không có `required`). Cập nhật Bug_Report B009/B010 với Actual thật thay placeholder dự đoán.
- **Human Review Notes:** Đã lưu đủ 4 ảnh — B010.png (tên chỉ khoảng trắng), B009-1/2/3.png (giá 0/âm/rỗng) — vào `FR-15_bugs/`. Đồng ý với cách AI gộp 3 trường hợp giá lỗi vào chung 1 bug ID (B009) vì cùng nguyên nhân gốc, thay vì tách thành 3 bug riêng gây trùng lặp không cần thiết.

### [LOG-014] — FR-15 BVA + Robust hoàn tất 100%, phát hiện B015
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên xác nhận đã bổ sung đủ 4 ảnh B014; gửi kết quả BV-02..05 và 2 robust R01/R02.
- **AI Output:** BV-02/03/04 Pass (đúng khớp spec ở các điểm biên hợp lệ). BV-05 (name 256 ký tự) Fail — phát hiện bug MỚI **B015**: hệ thống không truncate cũng không từ chối, lưu nguyên vẹn 256 ký tự — bổ sung khả năng thứ 3 (hoàn toàn không validate) mà thiết kế BVA ban đầu chỉ tính đến 2 khả năng (từ chối/cắt bớt). BV-01 đánh dấu Fail dùng chung bằng chứng với DT-05/B009 (không lặp lại test). Robust R01 (tràn INTEGER) và R02 (chặn ký tự chữ) đều Pass, không phát hiện bug mới. Cập nhật Metrics FR-15 cuối: 15/15 executed, 8 pass, 7 fail, 4 bugs (B009, B010, B014, B015). Bổ sung 2 bài học AI Gap Analysis (kiểm tra 2 tầng backend+client khi test Update; thiết kế Expected có đủ 3 khả năng khi spec không rõ hành vi vi phạm).
- **Human Review Notes:** Đã lưu ảnh BV-05 (tên 256 ký tự vẫn lưu nguyên vẹn) vào `FR-15_bugs/B015.png`. Đã tạo đủ 4 GitHub Issues (#11–#14) cho B009, B010, B014, B015 và dán link vào Bug_Report.md. Đồng ý với nhận xét của AI rằng thiết kế BVA ban đầu thiếu khả năng thứ 3 (hoàn toàn không validate) — đây là bài học thật, không phải chỉ AI tự nêu suông.

### [LOG-015] — FR-02 Mobile hoàn tất 100% — CẢ 4 FEATURE ĐÃ XONG
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên xác nhận kết quả robust test (empty submit không chặn, không crash); yêu cầu hoàn thiện FR-02 Mobile.
- **AI Output:** Xác nhận B016 (thiếu chặn submit rỗng + thiếu dấu * bắt buộc, severity thấp). Điền đầy đủ bảng cross-check với Feature A (số lần khóa/thời gian/thông báo — cả 3 đều nhất quán giữa web và mobile, xác nhận bug nằm ở backend dùng chung). Cập nhật Metrics FR-02 Mobile: 10 designed, 9 executed, 4 pass, 5 fail, 1 not-run (bỏ qua có chủ đích), 3 bug (B001 tái lập, B011 và B016 mới). Bổ sung 3 bài học AI Gap Analysis đặc trưng cho cross-platform testing (đọc code hời hợt vs thiếu dữ kiện, độ ưu tiên test cross-check khi đã có bằng chứng mạnh, không mặc định 2 client giống nhau ở tầng UI).
- **Human Review Notes:** Đã lưu ảnh B016 (màn hình submit rỗng không bị chặn) vào `FR-02-mobile_bugs/B016.png`. Đã tạo GitHub Issues (#15, #16) cho B011 và B016, dán link vào Bug_Report.md. Đồng ý để FR02M-BV-04 ở trạng thái "Not run có chủ đích" tại thời điểm này — sau đó tự quyết định quay lại test bổ sung ở LOG-018 vì thấy giá trị bằng chứng cross-platform là đáng làm thêm.

## TỔNG KẾT AUDIT — CẢ 4 FEATURE ĐÃ HOÀN THÀNH 100% VỀ NỘI DUNG KỸ THUẬT
- FR-02 (web): 21/21 TC, 6 bug (B001-B005, B012)
- FR-09: 20/20 TC, 4 bug (B006-B008, B013)
- FR-15: 15/15 TC, 4 bug (B009, B010, B014, B015)
- FR-02 Mobile: 9/10 TC (1 bỏ qua có chủ đích), 3 bug (B001 tái lập, B011, B016)
- Tổng: 65 TC thực thi, 17 bug ID xác nhận bằng chứng thật
- Việc còn lại: tạo GitHub Issues cho toàn bộ bug, AI_Critique.md, README Test Summary, xuất PDF, git log, đóng gói nộp bài.

### [LOG-018] — Xác nhận thêm B002 tái lập trên mobile (FR02M-BV-04)
- **Tool:** Claude Code (Sonnet 5)
- **Date & Time:** 2026-07-09
- **Prompt:** Sinh viên tự chủ động test lại FR02M-BV-04 (đã được đề xuất bỏ qua trước đó) — xác nhận sau đúng 3 phút đăng nhập lại thành công, không mở khóa sớm ở giây 30 như spec.
- **AI Output:** Cập nhật FR02M-BV-04 từ "Not run" thành "Fail — B002" (khớp y hệt kết quả web). Cập nhật bảng cross-check với Feature A: cả B001 lẫn B002 giờ đều được tự đo lại độc lập trên mobile, không còn chỉ là suy luận. Cập nhật Metrics FR-02 Mobile: 10/10 executed (100%), 4 pass, 6 fail, 0 not-run, 4 bug (B001, B002 tái lập + B011, B016 mới). Cập nhật bảng Tổng kết toàn bài: 66/66 test case đã thực thi (100%), 0 not-run. Sửa lại 1 điểm AI Gap Analysis đã lỗi thời (trước đó phê bình việc "bỏ qua BV-04" — nay không còn đúng vì sinh viên đã tự làm, viết lại bài học theo hướng "nên so sánh chi phí thực hiện thêm với giá trị tăng thêm thay vì mặc định bỏ qua").
- **Human Review Notes:** Đã có ảnh chụp lúc đăng nhập lại thành công sau đúng 3 phút, lưu bổ sung vào `FR-02-mobile_bugs/` dùng chung mã B002. Đây là bước sinh viên chủ động làm thêm ngoài đề xuất ban đầu của AI (AI từng gợi ý có thể bỏ qua để tiết kiệm thời gian) — quyết định tự làm để có bằng chứng đối chiếu cross-platform đầy đủ hơn, giúp Metrics FR-02 Mobile đạt 100% executed thay vì còn 1 case bỏ ngỏ.

