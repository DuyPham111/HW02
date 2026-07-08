# GUIDE — Hướng dẫn làm HW02 Domain Testing từng bước

> Đọc file này song song với `HW02-Domain-Testing-Roadmap-Revised (1).md` (lộ trình đã chốt).
> Guide này trả lời: **chạy cái gì, gõ lệnh gì, prompt AI ra sao, chụp gì, commit gì, điền vào file nào.**
>
> Bộ 4 feature: **FR02** (Pool A, web) · **FR09** (Pool B) · **FR15** (Pool C) · **FR02-Mobile** (Pool D).

---

## 0. Cấu trúc bài nộp (đã tạo sẵn — chỉ việc điền)

```
HW02-Submission/
├── guide.md                  ← file này (KHÔNG nộp, xóa trước khi zip)
├── README.md                 ← Test Summary + Self-Assessment + link video   [điền cuối cùng]
├── Main_Report.md            ← Báo cáo chính (ghép từ features/ lúc đóng gói)
├── bug-report.md             ← Mọi bug: 1 block/bug + link GitHub Issue
├── ai-audit-report.md        ← Log MỌI lần dùng AI (điền NGAY sau mỗi phiên)
├── ai-critique.md            ← 200–300 từ, viết cuối cùng
├── git-commit-log.txt        ← sinh bằng lệnh ở Bước 8
├── features/
│   ├── FR02-Login-Lockout/          ← mỗi feature 4 file + screenshots/
│   │   ├── 01-domain-testing.md     (BA spec-vs-code + partitions + TC domain)
│   │   ├── 02-bva.md                (bảng biên + TC BVA)
│   │   ├── 03-execution.md          (kết quả chạy + bằng chứng)
│   │   ├── 04-ai-gap-analysis.md    (AI sót gì + vì sao + sửa prompt)
│   │   └── screenshots/
│   ├── FR09-Discount-Coupons/       (cùng cấu trúc)
│   ├── FR15-Product-CRUD/           (cùng cấu trúc)
│   └── FR02-Mobile-Login-Lockout/   (cùng cấu trúc)
└── agent-skills/
    ├── domain-testing-skill.md
    ├── bva-skill.md
    ├── spec-vs-code-skill.md
    └── demo-video-link.txt
```

**Vì sao cấu trúc này?** Lai từ 2 bài mẫu: per-feature tách file theo step (bài QA-main — commit "1 step = 1 commit" rất tự nhiên) + bug report dạng block đầy đủ và AI Audit có Human Review Notes (bài Software-Testing-main — bài tự chấm 100). Toàn bộ Markdown, không LaTeX.

---

## ✅ 1. Phạm vi & repo (ĐÃ XÁC NHẬN — 2026-07-08)

1. **Phạm vi:** giảng viên xác nhận *HW02 là Functional Testing, bắt đầu tương tác từ web/mobile UI*. → **Mọi test case phải thao tác từ giao diện** (trang web hoặc app mobile). **KHÔNG** thiết kế TC gọi API trực tiếp bằng Postman/curl. DevTools (tab Network) chỉ được dùng để **quan sát** response làm bằng chứng đối chiếu (vd so message backend trả về với text app hiển thị) — không phải để tạo request.
2. **Repo GitHub Issues:** https://github.com/DuyPham111/HW02/issues (repo cá nhân). Remote của repo bài làm: `https://github.com/DuyPham111/HW02.git`.

---

## 2. Setup môi trường (Ngày 1 — ~1h)

### 2.1 Chạy SUT

```powershell
# Terminal 1 — Backend (cổng 3000)
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\backend"
npm install
node database.js     # seed/reset DB — CHẠY LẠI trước mỗi vòng test có state!
node server.js

# Terminal 2 — Web user (cổng 5173) — dùng cho FR02
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\frontend-web"
npm install
npm run dev

# Terminal 3 — Web admin (cổng 5174) — dùng cho FR15 (và tạo product mồi cho FR09)
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\frontend-admin"
npm install
npm run dev
```

Tài khoản seed: `admin@eshop.com / Admin123!` (admin) · `test@eshop.com / Test1234!` (user).

### 2.2 Mobile (làm ở Ngày 8, nhưng TEST CHẠY ĐƯỢC ngay hôm nay để tránh rủi ro)

```powershell
# Lấy IP LAN của máy:
ipconfig    # tìm IPv4 Address, ví dụ 192.168.1.5
```

- Mở `eshop-sut-main\frontend-mobile\App.js`, tìm `API_URL` và sửa thành `http://<IP-LAN>:3000/api`.
- Cài **Expo Go** trên điện thoại (cùng Wi-Fi với máy tính), rồi:

```powershell
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\eshop-sut-main\frontend-mobile"
npm install
npx expo start
```

- Quét QR bằng Expo Go. Nếu không kết nối được: tắt firewall cho Node hoặc dùng `npx expo start --tunnel`.

### 2.3 Git

```powershell
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\HW02-Submission"
git init
git add .
git commit -m "[setup] Add submission skeleton and report templates"
```

Quy ước commit (1 step = 1 commit): `[FR02][domain]`, `[FR02][bva]`, `[FR02][exec]`, `[FR02][bug]`, `[FR02][gap]`, `[FR09][domain]`… và `[skills]`, `[audit]`, `[final]`.

---

## 3. Quy trình chuẩn cho MỖI feature (lặp 4 lần)

> Mỗi feature = 6 bước → 5–6 commit. Với mỗi bước: **prompt AI → tự review → sửa → điền file → ghi log audit → commit.**

### Bước 1 — Business Analysis: Spec vs Code → điền mục 1 của `01-domain-testing.md`

Prompt mẫu (dán spec FR-XX từ `eshop-sut-main/README.md` + code handler từ `backend/server.js`):

```
Đây là spec của FR-XX: [dán spec]
Đây là code handler thực tế: [dán code]
Hãy liệt kê business rule THỰC TẾ từ code thành bảng
| Rule | Code thực tế (kèm dòng) | Khớp spec? |
và chỉ ra mọi điểm code khác spec. Không suy diễn rule không có trong code.
```

**Human review bắt buộc:** tự chạy app xác nhận từng điểm khác biệt AI nêu (đây là chỗ AI hay bịa). Ghi vào Human Review Notes.

→ Commit: `[FRxx][domain] Add business analysis (spec vs code)`

### Bước 2 — Domain Model + Domain TC → điền mục 2–5 của `01-domain-testing.md`

```
Với các biến input [liệt kê], hãy:
1) Xác định miền hợp lệ / không hợp lệ cho từng biến.
2) Chia equivalence partitions: | Biến | Partition ID | Mô tả | Giá trị đại diện | Valid? |
3) Sinh test case Domain theo nguyên tắc: MỖI TC chỉ 1 partition invalid,
   các biến khác giữ đại diện valid. Mọi TC phải thao tác được từ UI [tên trang].
   Format: | ID | Partition phủ | Thao tác trên UI | Expected theo spec | Ghi chú |
ID theo dạng FRxx-DT-01.
```

**Review checklist (tick trong file):** có partition rỗng/whitespace/Unicode/ký tự đặc biệt cho biến chuỗi? Có biến ẩn (trạng thái khóa, trạng thái auth)? Expected có xác định (không "hệ thống xử lý phù hợp")?

→ Commit: `[FRxx][domain] Add domain model and domain test cases`

### Bước 3 — BVA → điền `02-bva.md`

```
Với các biến có biên sau: [biến + min/max + nguồn biên từ spec],
sinh bảng BVA 3-point cho từng biến: off(min-1), on(min), in, on(max), off(max+1).
BẮT BUỘC có on-point (giá trị đúng bằng biên). Mỗi giá trị thành 1 TC:
| ID | Biên | Giá trị | Thao tác trên UI | Expected theo spec |
ID dạng FRxx-BVA-01.
```

**Review:** on-point có mặt chưa? (bug FR09 lộ đúng tại `= min`). Có biên 0/âm/tràn INTEGER/ngày?

→ Commit: `[FRxx][bva] Add boundary value analysis test cases`

### Bước 4 — Thực thi → điền `03-execution.md` (thủ công, KHÔNG nhờ AI đoán kết quả)

1. `node database.js` để reset DB (bắt buộc với FR02 — counter, FR09 — usage count).
2. Chạy từng TC trên UI theo đúng cột "Thao tác". Ghi **Actual** nguyên văn thông báo trên màn hình.
3. TC FAIL → **chụp screenshot NGAY** (xem mục 4 dưới), lưu vào `screenshots/` của feature.
4. Chỉ dùng 3 trạng thái: `Pass` / `Fail` / `Not executed` (mục tiêu: Not executed = 0).

→ Commit: `[FRxx][exec] Add execution results (X pass / Y fail)`

### Bước 5 — Bug report + GitHub Issues → điền `bug-report.md` (file chung ở root)

- Mỗi cụm TC fail cùng nguyên nhân gốc = **1 bug** (vd 3 TC fail vì `>` thay `>=` → 1 bug).
- Điền block theo template có sẵn trong `bug-report.md` (ID: `BUG-A-01`, `BUG-B-01`…).
- Tạo GitHub Issue: title = title của bug, body = copy block, **kéo-thả ảnh trực tiếp vào issue** (GitHub tự tạo URL — tránh lỗi ảnh không render như bài mẫu). Chụp lại trang issue → lưu vào `screenshots/` để nhúng ngược vào bug-report.
- Dán URL issue vào block bug.

→ Commit: `[FRxx][bug] Report BUG-x-01..0N with GitHub issues`

### Bước 6 — AI Gap Analysis → điền `04-ai-gap-analysis.md`

So sánh: TC/bug cuối cùng **vs** những gì AI sinh ra lần đầu. AI sót gì? Prompt lỗi, AI giới hạn, hay feature phức tạp? Đề xuất prompt tốt hơn. (Gợi ý gap có sẵn trong từng file template.)

→ Commit: `[FRxx][gap] Add AI gap analysis`

### Sau mỗi phiên AI — ghi `ai-audit-report.md` NGAY (đừng dồn cuối!)

Copy block template trong file đó, điền: tool, ngày giờ, prompt nguyên văn, tóm tắt output, **Human Review Notes** (bạn đã sửa gì). Đây là chỗ bài mẫu điểm cao ăn điểm.

---

## 4. Chụp screenshot đúng cách

- **Chụp màn hình app thật** đang thể hiện lỗi (không chụp text mô tả, không chụp code).
- Trong khung hình phải thấy: URL/màn hình nào + dữ liệu nhập + thông báo lỗi/kết quả sai.
- Đặt tên: `FAIL-<TCID>-<mô-tả-ngắn>.png` → vd `FAIL-FR02-BVA02-locked-after-2-attempts.png`.
- Công cụ: `Win+Shift+S` (Snipping Tool). Với lỗi khóa tài khoản: chụp kèm đồng hồ/timestamp để chứng minh thời gian khóa 3 phút.
- Mobile: chụp màn hình điện thoại hoặc cửa sổ emulator; quay thêm 1 video ngắn chuỗi "sai lần 1 → sai lần 2 → bị khóa" (dùng làm bằng chứng + tư liệu video demo).

---

## 5. Mẹo thực thi riêng từng feature

### FR02 (web) — `frontend-web` :5173, trang Đăng nhập
- Trước MỖI kịch bản đếm số lần sai: `node database.js` (Ctrl+C server → reset → chạy lại server).
- Đếm bằng tay số lần sai đến khi khóa (kỳ vọng spec: 3, thực tế sẽ khác → đó là bug). Đo thời gian khóa bằng đồng hồ (spec: 30s).
- Email không tồn tại vs mật khẩu sai: đối chiếu 2 thông báo có giống nhau không.

### FR09 — trang Checkout của `frontend-web`
- **Mẹo quan trọng:** để có tổng đơn đúng biên (299.999 / 300.000 / 300.001), vào **admin UI tạo product mồi** với giá đúng bằng các giá trị đó, rồi thêm 1 sản phẩm vào giỏ. Không cần Postman.
- Test lượt dùng VIP100 (max 2): áp mã → checkout thành công → áp lại. Reset DB khi cần làm lại.
- Quan sát kỹ số "Tiết kiệm"/"Thành tiền" sau khi áp SAVE10 — so với tính tay.

### FR15 — `frontend-admin` :5174, tab Sản phẩm
- Test validation từ UI: name rỗng, giá `0`, giá `-1`, giá chữ, giá cực lớn `2147483647`.
- Access control từ UI: **đăng nhập admin app bằng `test@eshop.com`** (user thường) → thử vào tab Sản phẩm và tạo/sửa/xóa. Nếu làm được → bug quan sát từ UI.
- Phạm vi UI-only → **không** dùng ca "gọi POST /api/products không token" bằng Postman. Bug access control (nếu có) phải thể hiện qua thao tác trên giao diện: user thường vào được admin UI và CRUD được sản phẩm.

### FR02-Mobile — Expo Go
- Chạy lại đúng bảng boundary của FR02 nhưng thao tác trên màn hình Login của app.
- Trọng tâm khác biệt: app hiển thị **cùng 1 câu lỗi chung** cho cả "sai mật khẩu" lẫn "bị khóa" → mở song song web (hoặc DevTools network) để đối chiếu message gốc backend. Đây là bug mobile-specific.
- Không copy nguyên văn báo cáo FR02 web — trình bày dạng **cross-check** + phần riêng mobile.

---

## 6. Agent Skills (10 điểm) + video

- 3 file trong `agent-skills/` đã viết sẵn dạng prompt-template — đọc, chỉnh theo cách bạn thực sự đã dùng.
- Quay 1 video (~5–10 phút, YouTube unlisted): mở skill → dán prompt cho 1 feature (chọn **FR09** vì bug rõ) → AI sinh TC → bạn review sửa → chạy 1–2 TC trên UI → thấy bug. Dán link vào `agent-skills/demo-video-link.txt` và `README.md`.
- (Nâng cao, khuyến khích: chuyển 3 file thành skill Claude Code thật trong `.claude/skills/<tên>/SKILL.md` — bài mẫu điểm 100 làm vậy.)

---

## 7. Tổng hợp cuối (Ngày 9–10)

1. **`ai-critique.md`**: trả lời 3 câu hỏi theo khung có sẵn, 200–300 từ (đếm từ!), dùng ví dụ thật từ bài (gap FR09 on-point, FR15 auth, mobile che message).
2. **Ghép Main_Report.md** từ các file feature:

```powershell
cd "D:\Nam3\HK3\Kiểm thử phần mềm\HW02-new\HW02-Submission"
$parts = @("Main_Report.md") + (Get-ChildItem features -Recurse -Filter "0*.md" | Sort-Object FullName | ForEach-Object FullName)
Get-Content $parts -Raw | Set-Content Main_Report_Full.md
```

   (`Main_Report.md` là phần mở đầu; file ghép `Main_Report_Full.md` dùng để xuất PDF.)
3. **Điền README.md**: bảng Test Summary (đếm từ các file execution) + Self-Assessment + link video.
4. **Xuất PDF** (cần cho Main report + AI Audit + AI Critique): mở file .md trong VS Code → extension **Markdown PDF** → `Export (pdf)`. Hoặc: `npx md-to-pdf Main_Report_Full.md`. Kiểm tra ảnh hiển thị trong PDF.
5. **Git log**:

```powershell
git log --pretty=format:"%h | %ad | %s" --date=format:"%Y-%m-%d %H:%M" > git-commit-log.txt
git add -A; git commit -m "[final] Package submission"
```

6. **Xóa `guide.md`**, zip cả thư mục, đặt tên theo format đề (kiểm tra lại trên Moodle), dạng tham khảo: `<MSSV>_HW02_AI_DomainTesting_<điểm-tự-chấm-3-chữ-số>.zip`.
7. Rà lần cuối bằng PHẦN 11 của roadmap (checklist nộp bài).

---

## 8. Lịch làm việc đề xuất (bám roadmap, ~10h)

| Buổi | Việc | File đầu ra |
|---|---|---|
| 1 (1h) | Setup + chạy đủ 4 app + xác nhận 2 việc ở mục 1 | commit `[setup]` |
| 2 (1.5h) | FR02: Bước 1–3 | `01`, `02` của FR02 |
| 3 (1.5h) | FR02: Bước 4–6 + issues | `03`, `04`, bug-report |
| 4 (1.5h) | FR09: Bước 1–3 (nhớ tạo product mồi) | `01`, `02` của FR09 |
| 5 (1.5h) | FR09: Bước 4–6 | `03`, `04`, bug-report |
| 6 (1.5h) | FR15: cả 6 bước | 4 file FR15 |
| 7 (1.5h) | Mobile: cross-check + bug mobile + video ngắn | 4 file FR02-Mobile |
| 8 (1h) | Skills + video demo + audit/critique | agent-skills/, ai-critique |
| 9 (1h) | Ghép report, PDF, README, git log, zip | gói nộp |

> **Nguyên tắc xuyên suốt:** không nộp raw AI output — mọi bảng AI sinh ra đều phải có dấu vết bạn đã sửa (Human Review Notes). Thiếu 1 tài liệu bắt buộc = 0 điểm, nộp trễ = 0 điểm.
