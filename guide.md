# GUIDE — Hướng dẫn làm HW02 Domain Testing từng bước

> Bộ 4 feature: **FR-02** (Pool A, web) · **FR-09** (Pool B) · **FR-15** (Pool C) · **FR-02 Mobile** (Pool D).
> Format báo cáo theo chuẩn 6-bước Domain / 4-bước BVA (tham khảo cấu trúc bài của các bạn cùng lớp,
> **nội dung và prompt phải tự viết** — đề phạt 0 điểm cả 2 bên nếu copy, kể cả copy prompt).

---

## 0. Cấu trúc bài nộp

```
HW02-Submission/
├── guide.md                     ← file này (KHÔNG nộp, xóa trước khi zip)
├── README.md                    ← Test Summary + Self-Assessment + link video [điền cuối]
├── reports/
│   ├── Main_Report.md           ← BÁO CÁO CHÍNH: 4 chương feature, mỗi chương 4 mục
│   │                               (Domain 6 bước / BVA 4 bước / Execution / AI Gap)
│   ├── Bug_Report.md            ← Bug hợp nhất: 1 block/bug + GitHub Issue
│   ├── AI_Audit_Report.md       ← Log MỌI phiên AI (điền NGAY sau mỗi phiên)
│   ├── AI_Critique.md           ← 200–300 từ, viết cuối
│   ├── git_commit_log.txt       ← sinh bằng lệnh ở mục 7
│   ├── FR-02_bugs/              ← ảnh bằng chứng: BUG-FR02-001.png, ...
│   ├── FR-09_bugs/  FR-15_bugs/  FR-02-mobile_bugs/
└── skills/                      ← Agent Skills (SKILL.md có frontmatter)
    ├── hw02-domain-testing/SKILL.md
    ├── hw02-bva/SKILL.md
    ├── hw02-spec-vs-code/SKILL.md
    └── demo-video-link.md
```

Mọi thứ của 1 feature nằm trong **1 chương của `reports/Main_Report.md`** — không còn file rời từng feature. Commit theo step = commit sau khi điền xong từng mục của chương.

## ✅ 1. Phạm vi & repo (ĐÃ XÁC NHẬN — 2026-07-08)

1. **Phạm vi:** Functional Testing bắt đầu tương tác từ **web/mobile UI**. KHÔNG thiết kế TC gọi API bằng Postman/curl. DevTools (Network) chỉ để **quan sát** response làm bằng chứng đối chiếu.
2. **Repo GitHub Issues:** https://github.com/DuyPham111/HW02/issues · remote: `https://github.com/DuyPham111/HW02.git`

---

## 2. Môi trường (ĐÃ SETUP XONG — chỉ cần khởi động lại khi làm việc)

```powershell
# Terminal 1 — Backend (đã npm install, DB đã seed)
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\backend"
node database.js     # reset DB — CHẠY LẠI trước mỗi vòng test có state (đếm lần sai, lượt coupon)
node server.js       # :3000

# Terminal 2 — Web user (:5173) — FR-02, FR-09
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\frontend-web" ; npm run dev

# Terminal 3 — Web admin (:5174) — FR-15 (và tạo product mồi cho FR-09)
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\frontend-admin" ; npm run dev

# Mobile (ngày làm Feature D): API_URL đã sửa = http://192.168.1.12:3000/api (nếu đổi Wi-Fi → ipconfig sửa lại)
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\frontend-mobile" ; npx expo start
```

Tài khoản seed: `admin@eshop.com / Admin123!` · `test@eshop.com / Test1234!`.

Quy ước commit (1 step = 1 commit): `[FR02][domain]`, `[FR02][bva]`, `[FR02][exec]`, `[FR02][bug]`, `[FR02][gap]`, tương tự FR09/FR15/FR02M, và `[skills]`, `[audit]`, `[final]`.

---

## 3. Quy trình mỗi feature (điền vào chương tương ứng trong `reports/Main_Report.md`)

> **QUAN TRỌNG — test case đã được AI sinh sẵn.** Thầy cho phép AI hỗ trợ sinh test case, nên phần
> **Domain (Bước 1–6) và BVA của cả 4 feature đã điền đầy đủ giá trị cụ thể** trong `Main_Report.md`
> (cột Expected theo spec; cột **Actual/Pass-Fail để trống**). Việc chính của bạn giờ là **BƯỚC 4–6**:
> chạy thật từng TC trên UI → điền Actual → chụp ảnh → viết bug. Bước 1–3 chỉ cần **đọc lại & kiểm chứng**,
> không phải viết từ đầu.

| # | Bước | Trạng thái | Việc của bạn |
|---|------|-----------|--------------|
| 1 | Business Analysis (spec-vs-code) | ✅ AI đã điền | Đọc lại; chạy checklist kiểm chứng tay (cuối Bước 1 mỗi chương) để xác nhận code-vs-spec đúng |
| 2 | Domain: Input/Domains/EP/Constraints/TC | ✅ AI đã điền | Đọc lại, kiểm tra TC có hợp lý không; sửa nếu thấy sai |
| 3 | BVA: Boundaries/Points/TC/Robust | ✅ AI đã điền | Đọc lại |
| 4 | **Thực thi TOÀN BỘ TC trên UI** | ⬜ **BẠN LÀM** | Chạy từng TC, điền cột Actual + Pass/Fail, chụp ảnh mọi ca Fail |
| 5 | Bug report + GitHub Issues | ⬜ **BẠN LÀM** | Xác nhận bug thật, điền Actual vào `Bug_Report.md`, tạo Issue + ảnh |
| 6 | AI Gap Analysis + Metrics | ⬜ **BẠN LÀM** | Điền bảng Metrics; viết gap (gợi ý có sẵn trong comment) |

**Commit tương ứng:** `[FRxx][exec] Add execution results`, `[FRxx][bug] Report bugs with issues`, `[FRxx][gap] Add AI gap analysis`.

> Nếu muốn AI sinh lại/bổ sung test case: dùng skill trong `skills/` (đã cho phép sinh giá trị cụ thể).
> Luôn **tự chạy để xác nhận** trước khi tin — AI chỉ đoán Expected, không biết Actual.

**Nguyên tắc thực thi (bước 4):**
- Reset DB trước kịch bản có state. Actual ghi **nguyên văn** text trên UI + số liệu đo.
- Chỉ dùng `Pass` / `Fail` / `Not run` (mục tiêu Not run = 0 — thà thiết kế ít mà chạy hết).
- TC Fail → chụp screenshot NGAY, lưu `reports/FR-XX_bugs/BUG-FRXX-00N.png` (gộp TC fail cùng nguyên nhân = 1 bug).

**Bug (bước 5):** block theo mẫu trong `Bug_Report.md`. Tạo GitHub Issue: Title `[FR-XX] [BUG-FRXX-00N] ...`, **kéo-thả ảnh trực tiếp vào issue**, dán Link ngược vào block. Chụp lại trang issue nếu muốn nhúng vào báo cáo.

**Audit (mọi bước):** mỗi phiên AI 1 block trong `AI_Audit_Report.md` — tool, giờ, prompt nguyên văn, tóm tắt output, **Human Review Notes** (đã kiểm chứng/sửa gì).

---

## 4. Chụp screenshot đúng cách

- Chụp **màn hình app thật** thể hiện lỗi (không chụp text mô tả/code). Khung hình thấy: URL/màn hình + dữ liệu nhập + kết quả sai.
- Tên file: `B00N.png` (khớp Defect ID trong `Bug_Report.md`) trong thư mục `reports/FR-XX_bugs/`. `Win+Shift+S`.
- Lỗi khóa tài khoản: chụp kèm đồng hồ/timestamp để chứng minh thời gian.
- Mobile: chụp màn hình điện thoại; quay thêm video ngắn chuỗi "sai lần 1 → 2 → khóa" (bằng chứng + tư liệu video demo). Video: để file `.mov/.mp4` trong thư mục bugs + 1 ảnh preview (PDF không nhúng video).

---

## 5. Mẹo thực thi riêng từng feature

### FR-02 (web) — :5173/login
- Reset DB trước MỖI kịch bản đếm. Đếm tay số lần sai đến khi khóa (spec: 3; dự đoán: 2). Đo thời gian khóa bằng đồng hồ (spec: 30s; dự đoán: ~3 phút).
- Quan sát text lỗi trên UI (dự đoán: luôn 1 câu chung); đối chiếu response 401/403 qua DevTools Network.
- Bonus UI-only đã xác minh code: ô email `type="text"`, ô **mật khẩu hiện rõ ký tự**, tiêu đề "Đăng Ký", lỗi nằm dưới nút submit.

### FR-09 — Checkout :5173
- **Product mồi:** tạo qua admin UI sản phẩm giá 299.999 / 300.000 / 300.001 (SAVE10) và 499.999 / 500.000 (BIGBUY) → thêm 1 cái vào giỏ để có tổng đúng biên. Không cần Postman.
- VIP100 (max 2 lượt): áp → checkout thành công → áp lại → lần 3. Reset DB khi cần đếm lại.
- Sau khi áp SAVE10: so số "Tiết kiệm"/"Thành tiền" với tính tay — mã được nhận ≠ tính đúng tiền.

### FR-15 — Admin :5174, tab Sản phẩm
- Validation từ UI: name rỗng/255/256 ký tự (nhờ AI sinh chuỗi, paste), giá 0 / 1 / -1 / chữ / 2147483647 / 2147483648.
- Access control từ UI: **đăng nhập admin app bằng `test@eshop.com`** → thử vào tab Sản phẩm và Create/Update/Delete. Làm được = bug quan sát từ UI.
- KHÔNG dùng ca "gọi API không token" bằng Postman (ngoài phạm vi UI).

### FR-02 Mobile — Expo Go
- Chạy lại đúng bảng biên của Feature A trên màn hình Login app → **cross-check**: hành vi có tái lập y hệt không (bảng so sánh trong chương D).
- Đã xác minh code: cả web (`Login.jsx:18`) lẫn mobile (`App.js:205`) đều nuốt message backend → bug "không báo bị khóa" có ở **cả 2 client**; lấy response gốc bằng DevTools trên web song song.
- Bug backend tái hiện trên mobile: comment vào issue gốc, không tạo issue trùng.

---

## 6. Agent Skills (10đ) + video

- 3 skill trong `skills/` viết dạng SKILL.md chuẩn (frontmatter name/description). Chỉnh lại theo cách bạn THỰC SỰ dùng — đừng để nguyên nếu quy trình thực tế khác.
- (Nâng cao) Copy vào `.claude/skills/` của repo để Claude Code tự nhận và gọi bằng lệnh.
- Quay video demo end-to-end trên **FR-09** theo kịch bản trong `skills/demo-video-link.md`; dán link vào file đó + README.

---

## 7. Đóng gói cuối (không còn bước ghép file — Main_Report đã là 1 file)

1. `AI_Critique.md`: 200–300 từ (đếm từ!) theo khung 3 câu hỏi có sẵn, ví dụ thật từ bài.
2. Điền README.md: Test Summary (cộng từ các bảng Metrics) + Self-Assessment + link video.
3. **Xuất PDF** từng file trong `reports/` (Main_Report, Bug_Report, AI_Audit_Report, AI_Critique): VS Code extension **Markdown PDF** → `Export (pdf)`, hoặc `npx md-to-pdf <file>`. Kiểm tra ảnh hiển thị trong PDF.
4. Git log:
```powershell
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\HW02-Submission"
git log --pretty=format:"%h | %ad | %s" --date=format:"%Y-%m-%d %H:%M" > reports/git_commit_log.txt
git add -A ; git commit -m "[final] Package submission" ; git push -u origin main
```
5. Xóa `guide.md`, zip thư mục, tên file theo quy định Moodle: `[MSSV]_HW02_AI_DomainTesting_[điểm 3 chữ số].zip`.
6. Rà checklist: đủ Main_Report(+pdf) / Bug_Report / Audit(+pdf) / Critique(+pdf) / git log / README / skills / video? Mỗi TC có Expected xác định? Mỗi issue có ảnh? **Thiếu 1 tài liệu = 0 điểm.**

---

## 8. Lịch đề xuất (~10h)

| Buổi | Việc | Commit |
|---|---|---|
| 1 (xong) | Setup + phạm vi + skeleton | `[setup]` ×3 |
| 2 (1.5h) | FR-02: kiểm chứng tay Bước 1 → Bước 2–6 Domain → BVA | `[FR02][domain]` ×2, `[FR02][bva]` |
| 3 (1.5h) | FR-02: Exec + Bug + Issues + Gap | `[FR02][exec/bug/gap]` |
| 4 (1.5h) | FR-09: spec-vs-code → Domain → BVA (tạo product mồi) | `[FR09][domain/bva]` |
| 5 (1.5h) | FR-09: Exec + Bug + Gap | `[FR09][exec/bug/gap]` |
| 6 (1.5h) | FR-15: cả 6 bước | `[FR15]...` |
| 7 (1.5h) | Mobile: cross-check + bug mobile + video ngắn | `[FR02M]...` |
| 8 (1h) | Skills + video demo + Critique | `[skills]`, `[audit]` |
| 9 (1h) | README, PDF, git log, zip | `[final]` |

> **Nguyên tắc xuyên suốt:** không nộp raw AI output — mọi bảng AI sinh phải có dấu vết Human Review. Nộp trễ = 0. Copy giữa sinh viên (kể cả prompt) = 0 cả hai.
