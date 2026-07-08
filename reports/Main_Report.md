# Main Testing Report — HW02 Domain Testing on EShop

**Họ và tên:** [HỌ TÊN]
**MSSV:** [MSSV]
**Nhóm:** [Nhóm]
**Ngày thực hiện:** [từ ngày] → [đến ngày]
**SUT:** EShop — https://github.com/ttbhanh/eshop-sut
**Backend:** `http://localhost:3000/api` · Web: `:5173` · Admin: `:5174` · Mobile: Expo (`API_URL = http://192.168.1.12:3000/api`)

**Phạm vi thực thi:** Functional Testing bắt đầu tương tác từ **web/mobile UI** (xác nhận của giảng viên ngày 2026-07-08). Không có test case gọi API trực tiếp; DevTools (Network) chỉ dùng **quan sát** response làm bằng chứng đối chiếu.

## Tổng quan 4 feature

| Pool | Feature | Nơi test | Handler backend liên quan |
|---|---|---|---|
| A | FR-02 Đăng nhập & Khóa tài khoản | Trang Đăng nhập web (:5173/login) | `POST /api/login` |
| B | FR-09 Mã giảm giá | Trang Checkout web (:5173) | `POST /api/apply-coupon` |
| C | FR-15 Quản lý Sản phẩm (CRUD) | Web Admin (:5174), tab Sản phẩm | `POST/PUT/DELETE /api/products` |
| D | FR-02 Mobile (cross-check) | App Expo, màn hình Login | `POST /api/login` (cùng handler Pool A) |

**Quy trình chung mỗi feature:** Domain Testing (6 bước) → BVA (4 bước) → Test Execution → AI Gap Analysis. Mỗi bước 1 commit; mỗi phiên AI ghi vào `AI_Audit_Report.md`. Bug tổng hợp ở `Bug_Report.md`, ảnh bằng chứng ở `reports/FR-XX_bugs/`.

---
---

# FEATURE A: FR-02 — ĐĂNG NHẬP & KHÓA TÀI KHOẢN

## 1. Domain Testing — FR-02

**SUT:** Trang Đăng nhập web (`frontend-web/src/pages/Login.jsx`, `context/AuthContext.jsx`) + `POST /api/login` (`backend/server.js:32–66`) + DB state (`users.login_attempts`, `users.locked_until`)

### Bước 1 — Phạm vi và SUT

#### Tài liệu đặc tả (SRS FR-02)

| # | Rule | Trích spec |
|---|---|---|
| R1 | Mỗi lần sai tăng bộ đếm đúng 1 đơn vị | "Sau mỗi lần đăng nhập sai, hệ thống tăng bộ đếm lên **đúng 1 đơn vị**." |
| R2a | Sai ≥ 3 lần liên tiếp → khóa tài khoản | "Nếu đăng nhập sai từ **3 lần trở lên** liên tiếp, tài khoản bị tạm khóa…" |
| R2b | Thời gian khóa 30 giây (demo) | "…tạm khóa **30 giây** (môi trường demo)." |
| R3 | Thông báo lỗi phù hợp, không lộ nguyên nhân chi tiết | "Hệ thống trả về thông báo lỗi phù hợp; không để lộ chi tiết nguyên nhân." |
| R4 | Đăng nhập đúng → trả JWT (bộ đếm reset — suy ra từ "liên tiếp") | "Đăng nhập thành công trả về JWT Token…" |
| R5 | Ô email phải dùng `type="email"` (HTML5 validate) | "Trường email phải dùng `type=\"email\"`." |

#### Phân tích sơ bộ từ mã nguồn thực tế (Code vs SRS)

> Phân tích bởi AI, **trạng thái: chờ kiểm chứng bằng thực thi trên UI** (checklist cuối Bước 1).

| Rule | Code thực tế (file:dòng) | Khớp spec? |
|---|---|---|
| R1 | `const newAttempts = user.login_attempts + 2;` — `server.js:54` | ❌ Tăng **+2** mỗi lần sai |
| R2a | `if (newAttempts >= 3)` — `server.js:56`; do +2 nên: lần sai 1 → đếm 2 (chưa khóa), lần sai 2 → đếm 4 ≥ 3 (khóa) | ❌ Thực tế khóa sau **2 lần** sai |
| R2b | `lockedUntil = new Date(Date.now() + 180000)` — `server.js:57` | ❌ Khóa **180s = 3 phút**, không phải 30s |
| R3 | Backend: cùng câu "Invalid email or password" cho email-không-tồn-tại (`server.js:38`) lẫn sai mật khẩu (`server.js:63`) ✅; bị khóa → 403 "Tài khoản đã bị khóa. Vui lòng thử lại sau." (`server.js:41–43`). **Nhưng** `Login.jsx:17–19` catch mọi lỗi, luôn hiện "Đăng nhập thất bại. Vui lòng kiểm tra lại." | ⚠️ Backend ✅ / **UI ❌** — người bị khóa thấy y hệt câu sai mật khẩu |
| R4 | Reset `login_attempts = 0, locked_until = NULL` (`server.js:47–49`), trả JWT (`server.js:51–52`) | ✅ |
| R5 | Ô email `type="text"` — `Login.jsx:30` | ❌ Không có HTML5 validate |

**Quan sát UI bổ sung trên trang Đăng nhập** (trái FR-21/FR-22, quan sát được ngay trong feature này):

| Quan sát | Code | Spec liên quan |
|---|---|---|
| Ô mật khẩu `type="text"` → **mật khẩu hiển thị rõ** khi gõ | `Login.jsx:40` | FR-22: phải `type="password"` |
| Tiêu đề trang Đăng nhập ghi "Đăng Ký" | `Login.jsx:24` | FR-21 |
| Label "Username" thay vì "Email"; nút "Sign In" (tiếng Anh) | `Login.jsx:28,58` | FR-21 nhất quán ngôn ngữ |
| Thông báo lỗi hiển thị **dưới** nút submit | `Login.jsx:66` | FR-22: lỗi phải nằm **trên** nút submit |
| 2 ô input có `required` → bỏ trống bị trình duyệt chặn | `Login.jsx:34,44` | ảnh hưởng partition "rỗng" |

**Checklist kiểm chứng bằng tay trên UI (làm trước khi thực thi chính thức):**

- [ ] Reset DB (`node database.js`) → đăng nhập sai bằng `test@eshop.com`: đếm số lần đến khi bị khóa (dự đoán: 2), chụp màn hình từng lần
- [ ] Ngay khi khóa: đăng nhập với mật khẩu **đúng** → vẫn bị chặn? Ghi nguyên văn câu hiển thị
- [ ] Bấm giờ: thử mật khẩu đúng ở mốc ~30s và ~3 phút → ghi thời điểm vào lại được
- [ ] Nhập email `abc` → form có bị HTML5 chặn không? (dự đoán: không)
- [ ] Gõ mật khẩu → ký tự hiện rõ? Chụp màn hình
- [ ] F12 → Network → request `login`: đối chiếu status 401/403 + JSON `error` với text UI

### Bước 2 — Input Variables

| ID | Biến | Kiểu | Nguồn | Ràng buộc SRS |
|----|------|------|-------|---------------|
| V1 | `email` | String | UI form / API body | Định dạng hợp lệ; ô nhập dùng `type="email"` |
| V2 | `password` | String | UI form / API body | Khớp mật khẩu user trong DB; ô nhập dùng `type="password"` |
| V3 | `login_attempts` (biến ẩn) | Integer | DB `users.login_attempts` | +1 mỗi lần sai; reset 0 khi đăng nhập đúng |
| V4 | `locked_until` (biến ẩn) | Datetime | DB `users.locked_until` | Được set (khóa 30s) khi sai ≥ 3 lần liên tiếp |

**Bộ giá trị hợp lệ mặc định (single-fault):** `test@eshop.com / Test1234!`, tài khoản active (attempts = 0, locked_until = null).

### Bước 3 — Domains

> AI Output — **chờ kiểm chứng bằng thực thi** (Bước 6 dưới đây chỉ chạy thật mới biết Actual).

| Biến | Valid Domain | Invalid Domain | Special |
|------|--------------|----------------|---------|
| `email` | Email đã đăng ký trong DB (`test@eshop.com`) | Email chưa đăng ký (`ghost@eshop.com`); email sai định dạng (`abc`, `abc@`, `abc@com`) | Rỗng (`""`); chỉ khoảng trắng (`"   "`); có khoảng trắng đầu/cuối (`" test@eshop.com "`); chuỗi SQL injection (`' OR 1=1--`) |
| `password` | Đúng mật khẩu của user (`Test1234!`) | Sai mật khẩu (`Wrong123!`) | Rỗng (`""`); rất dài (500 ký tự); ký tự đặc biệt/Unicode (`Mật_khẩu@123`) |
| `login_attempts` (ẩn) | 0, 1, 2 (theo spec — chưa đủ ngưỡng khóa) | ≥ 3 (theo spec — phải khóa) | — |
| `locked_until` (ẩn) | `null`, hoặc thời điểm đã trôi qua | Thời điểm còn hiệu lực (tương lai) | — |

### Bước 4 — Equivalence Partitions

| EP-ID | Loại vùng | Biến | Mô tả | Giá trị đại diện |
|-------|-----------|------|-------|------------------|
| EP-FR02-01 | Hợp lệ | email | Email đã đăng ký | `test@eshop.com` |
| EP-FR02-02 | Không hợp lệ | email | Email chưa đăng ký (đúng định dạng) | `ghost@eshop.com` |
| EP-FR02-03 | Không hợp lệ | email | Email sai định dạng | `abc-khong-phai-email` |
| EP-FR02-04 | Đặc biệt | email | Email rỗng | `""` |
| EP-FR02-05 | Hợp lệ | password | Mật khẩu đúng | `Test1234!` |
| EP-FR02-06 | Không hợp lệ | password | Mật khẩu sai | `Wrong123!` |
| EP-FR02-07 | Đặc biệt | password | Mật khẩu rỗng | `""` |
| EP-FR02-08 | Hợp lệ | login_attempts | Chưa đạt ngưỡng khóa (theo spec) | attempts ∈ {0,1,2} |
| EP-FR02-09 | Không hợp lệ (theo spec) | login_attempts | Đã đạt ngưỡng khóa | attempts ≥ 3 |
| EP-FR02-10 | Hợp lệ | locked_until | Không bị khóa | `null` hoặc đã hết hạn |
| EP-FR02-11 | Không hợp lệ | locked_until | Đang bị khóa | thời điểm tương lai |

### Bước 5 — Constraints (ràng buộc chéo / nghiệp vụ)

| C-ID | Ràng buộc | Loại |
|------|-----------|------|
| C-FR02-01 | Đăng nhập thành công → reset bộ đếm và mở khóa | dependency |
| C-FR02-02 | Mỗi lần thất bại → bộ đếm +1 đúng 1 đơn vị | business rule |
| C-FR02-03 | Sai ≥ 3 lần liên tiếp → khóa 30 giây | business rule |
| C-FR02-04 | Ô email `type="email"`, ô mật khẩu `type="password"` | GUI rule |
| C-FR02-05 | Người dùng bị khóa phải nhận thông báo bị khóa (khác câu sai mật khẩu) | business rule |
| C-FR02-06 | Tài khoản đang bị khóa (EP-FR02-11) thì dù `password` đúng (EP-FR02-05) vẫn phải bị từ chối | dependency (state override credential) |

### Bước 6 — Test Cases (Domain)

*Mặc định các biến còn lại giữ giá trị hợp lệ (single-fault). Mọi TC thao tác từ trang Đăng nhập http://localhost:5173/login. Cột Actual/Pass-Fail/Bug-ID để trống — chỉ điền sau khi CHẠY THẬT (mục 3. Test Execution).*

| TC-ID | Mô tả | Input / thao tác trên UI | Expected (SRS) | EP/Constraint | Actual | Pass/Fail | Bug-ID |
|-------|-------|--------------------------|----------------|---------------|--------|-----------|--------|
| FR02-DT-01 | Đăng nhập thành công (happy path) | `test@eshop.com` / `Test1234!`, bấm "Sign In" | Đăng nhập thành công, vào trang chủ, nhận JWT | EP-01, EP-05, EP-08, EP-10 | | | |
| FR02-DT-02 | Email chưa đăng ký | `ghost@eshop.com` / `Test1234!` | Từ chối, thông báo lỗi chung (không nói rõ "email không tồn tại") | EP-02, C-FR02-05 | | | |
| FR02-DT-03 | Email sai định dạng | `abc-khong-phai-email` / `Test1234!`, bấm "Sign In" | Client chặn (nếu có HTML5 validate) hoặc server từ chối | EP-03, C-FR02-04 | | | |
| FR02-DT-04 | Email bỏ trống | để trống ô email, nhập password, bấm "Sign In" | Trình duyệt chặn submit (thuộc tính `required`) | EP-04 | | | |
| FR02-DT-05 | Sai mật khẩu, lần đầu tiên | `test@eshop.com` / `Wrong123!` (tài khoản mới reset, attempts=0) | Từ chối; theo spec: `login_attempts` tăng lên **1**; tài khoản CHƯA bị khóa | EP-06, EP-08, C-FR02-02 | | | |
| FR02-DT-06 | Mật khẩu bỏ trống | nhập email đúng, để trống password, bấm "Sign In" | Trình duyệt chặn submit (thuộc tính `required`) | EP-07 | | | |
| FR02-DT-07 | Đăng nhập khi tài khoản đang bị khóa, dù mật khẩu ĐÚNG | Làm sai liên tục cho đến khi bị khóa, sau đó thử lại với `Test1234!` (đúng) | Vẫn bị từ chối; thông báo phải nói rõ "tài khoản đang bị khóa" (khác câu sai mật khẩu) | EP-05, EP-11, C-FR02-06, C-FR02-05 | | | |
| FR02-DT-08 | Đăng nhập đúng sau khi đã có vài lần sai (nhưng CHƯA bị khóa) | Sai 1 lần, sau đó đăng nhập đúng `Test1234!` | Đăng nhập thành công; bộ đếm reset về 0 | EP-05, EP-08, C-FR02-01 | | | |

### Tóm tắt coverage

- Số EP: 11 — Số TC: 8 — EP chưa cover: EP-FR02-09 (được phủ gián tiếp qua TC-07, và trực tiếp hơn ở BVA Bước 3 dưới)

---

## 2. Boundary Value Analysis — FR-02

### Bước 1 — Boundaries từ SRS

| B-ID | Biến | Ràng buộc | Min | Max | Kiểu |
|------|------|-----------|-----|-----|------|
| B-FR02-01 | `login_attempts` | Khóa khi sai từ 3 lần liên tiếp | 1 | 3 (ngưỡng khóa) | count |
| B-FR02-02 | Thời gian khóa | 30 giây | 30s | 30s | time |

### Bước 2 — BVA Points

> AI Output — giá trị "Expected" theo SPEC; giá trị thật (Actual) chỉ có sau khi chạy (Bước 3 dưới + mục 3. Test Execution). Dự đoán trước khi chạy (dựa trên đã đọc code ở Domain Bước 1): do bug `+2`, biên THẬT sẽ lộ ra ở lần sai thứ **2**, không phải thứ 3; thời gian khóa THẬT dự đoán ~180s (3 phút), không phải 30s.

| Point | Biến | Giá trị | Quan hệ biên |
|-------|------|---------|--------------|
| P-01 | `login_attempts` | 1 | min (mới sai 1 lần) |
| P-02 | `login_attempts` | 2 | in (giữa vùng hợp lệ, chưa khóa) |
| P-03 | `login_attempts` | 3 | **on-point** (đúng ngưỡng spec — quan trọng nhất) |
| P-04 | `login_attempts` | 4 | off-point (vượt ngưỡng, đã phải khóa từ trước) |
| P-05 | Thời gian sau khi khóa | 29 giây | off (trước khi hết hạn khóa — vẫn phải khóa) |
| P-06 | Thời gian sau khi khóa | 30 giây | **on-point** (đúng ngưỡng spec — vừa hết hạn khóa) |
| P-07 | Thời gian sau khi khóa | 31 giây | off (sau khi hết hạn khóa — phải mở) |

### Bước 3 — Test Cases (BVA)

*Reset DB (`node database.js`) trước khi bắt đầu chuỗi test đếm số lần sai — vì `login_attempts` là state lưu trong DB, không reset thì kết quả bị nhiễu bởi lần chạy trước.*

| TC-ID | Input / thao tác trên UI | Boundary | Expected (SRS) | Actual | Pass/Fail | Bug-ID |
|-------|--------------------------|----------|----------------|--------|-----------|--------|
| FR02-BV-01 | Sai mật khẩu lần thứ 1 (`test@eshop.com` / `Wrong123!`) | P-01 (attempts=1) | Từ chối; `login_attempts` = 1; **chưa** bị khóa | | | |
| FR02-BV-02 | Sai mật khẩu lần thứ 2 (liên tiếp) | P-02 (attempts=2) | Từ chối; `login_attempts` = 2; **chưa** bị khóa | | | |
| FR02-BV-03 | Sai mật khẩu lần thứ 3 (liên tiếp) | P-03 — on-point | Từ chối; **bị khóa ngay** (403), thông báo "tài khoản đã bị khóa" | | | |
| FR02-BV-04 | Thử đăng nhập (mật khẩu đúng hoặc sai) ngay sau lần sai thứ 3 | P-04 (attempts=4, đã khóa từ P-03) | Vẫn bị từ chối vì đang trong thời gian khóa | | | |
| FR02-BV-05 | Sau khi bị khóa ở BV-03, đợi đúng 29 giây rồi thử đăng nhập bằng mật khẩu ĐÚNG | P-05 | Vẫn bị từ chối — còn 1 giây nữa mới hết hạn khóa | | | |
| FR02-BV-06 | Đợi đến giây thứ 30 (tính từ lúc bị khóa) rồi thử đăng nhập bằng mật khẩu ĐÚNG | P-06 — on-point | Đăng nhập **thành công**, bộ đếm reset về 0 | | | |
| FR02-BV-07 | Đợi đến giây thứ 31 rồi thử đăng nhập bằng mật khẩu ĐÚNG | P-07 | Đăng nhập thành công (đã chắc chắn hết hạn khóa) | | | |

### Bước 4 — Robust / Edge (bổ sung)

| TC-ID | Input | Ghi chú |
|-------|-------|---------|
| FR02-BV-R01 | email + password đều rỗng | HTML5 `required` chặn? |
| FR02-BV-R02 | email có khoảng trắng thừa `"  test@eshop.com  "` | Server có tự trim trước khi so khớp DB không? |
| FR02-BV-R03 | email = `' OR 1=1--` | Kiểm tra SQL injection có gây lỗi 500 hay bị chặn an toàn |
| FR02-BV-R04 | password rất dài (500 ký tự) | Kiểm tra không bị crash/treo |

### Tóm tắt

- Số boundary: 2 (ngưỡng số lần sai; ngưỡng thời gian khóa) — Số TC: 11 (7 BVA chính + 4 robust) — Fail: [điền sau khi thực thi]

---

## 3. Test Execution — FR-02

**Ngày thực thi:** [.....] · **Môi trường:** Windows 11, Node v22, Chrome, web :5173, backend :3000 · **Reset DB** trước mỗi kịch bản đếm: `node database.js`

| TC-ID | Mô tả | Expected (SRS) | Actual (quan sát trên UI, nguyên văn) | Result | Bug-ID |
|-------|-------|----------------|----------------------------------------|--------|--------|
| | | | | | |

### Metrics — FR-02

| Designed | Executed | Pass | Fail | Not run | Bugs |
|----------|----------|------|------|---------|------|
| | | | | | |

---

## 4. AI Gap Analysis — FR-02

### 1. Kịch bản/lỗi AI bỏ sót (AI Gaps)

<!-- Gợi ý gap FR02: AI giả định counter +1 và khóa sau 3 lần theo "kiến thức chung", không tự biết
     hành vi thật nếu không được dán code; AI không nghĩ đến ĐO thời gian khóa thực tế;
     AI đoán message tiếng Anh trong khi cần đối chiếu text tiếng Việt hiển thị trên UI;
     AI bỏ qua thuộc tính GUI (type ô input, vị trí thông báo lỗi). -->

| Kịch bản / lỗi bị bỏ sót | Root cause (vì sao AI sót) | Bài học & khắc phục |
|--------------------------|----------------------------|---------------------|
| | | |

### 2. Cải tiến prompt

1. [prompt cũ → prompt cải thiện]

---
---

# FEATURE B: FR-09 — MÃ GIẢM GIÁ (DISCOUNT COUPONS)

## 1. Domain Testing — FR-09

**SUT:** Trang Checkout web (`frontend-web/src/pages/Checkout.jsx`) + `POST /api/apply-coupon` (`backend/server.js:363–441`) + bảng `coupons`, `coupon_usage`

### Bước 1 — Phạm vi và SUT

#### Tài liệu đặc tả (SRS FR-09) — 5 điều kiện + công thức

| # | Rule | Trích spec |
|---|---|---|
| C1 | Mã tồn tại và `is_active = 1` | |
| C2 | Ngày hiện tại trước `expired_at` | |
| C3 | Tổng đơn **>=** `min_order_amount` | |
| C4 | Đã đăng nhập (JWT hợp lệ) | |
| C5 | Số lần đã dùng < `max_uses_per_user` | |
| F1 | percent: `discount = total × value / 100` | |
| F2 | fixed: `discount = value`; `final = total − discount` | |

**Coupon seed (test data):** SAVE10 (percent 10, min 300.000) · BIGBUY (fixed 50k, min 500.000) · VIP100 (fixed 100k, min 300.000, max 2 lượt) · EXPIRED (percent 20, hết hạn 2020-01-01).

#### Phân tích sơ bộ từ mã nguồn thực tế (Code vs SRS)

<!-- Điền bằng skill spec-vs-code (dán handler /api/apply-coupon server.js:363-441 + Checkout.jsx).
     Chú ý kỹ: điều kiện so sánh ngưỡng dùng > hay >= (server.js:379)? công thức percent
     (server.js:399-401) có đúng total*value/100? C4 có được kiểm không (endpoint có authenticateToken?)?
     KIỂM CHỨNG TRÊN UI trước khi kết luận. -->

| Rule | Code thực tế (file:dòng) | Khớp spec? |
|---|---|---|
| C1 | | |
| C2 | | |
| C3 | | |
| C4 | | |
| C5 | | |
| F1 | | |
| F2 | | |

**Chuẩn bị dữ liệu UI-first:** để có tổng giỏ đúng biên (299.999 / 300.000 / 300.001…), dùng admin UI tạo **product mồi** với giá đúng các giá trị đó rồi thêm 1 sản phẩm vào giỏ — không cần gọi API.

### Bước 2 — Input Variables

| ID | Biến | Kiểu | Nguồn | Ràng buộc SRS |
|----|------|------|-------|---------------|
| V1 | `code` | String | Ô nhập mã ở Checkout | Tồn tại + `is_active = 1` |
| V2 | `total_amount` | Integer | Tổng giỏ hàng (điều khiển qua product mồi) | `>= min_order_amount` |
| V3 | trạng thái đăng nhập (biến ẩn) | State | Session / token client | Phải đăng nhập (C4) |
| V4 | số lượt đã dùng (biến ẩn) | Integer | DB `coupon_usage` | `< max_uses_per_user` |
| V5 | thời điểm hiện tại vs `expired_at` | Datetime | Hệ thống + thuộc tính coupon | Trước `expired_at` |

### Bước 3 — Domains

| Biến | Valid Domain | Invalid Domain | Special |
|------|--------------|----------------|---------|
| | | | |

### Bước 4 — Equivalence Partitions

| EP-ID | Loại vùng | Biến | Mô tả | Giá trị đại diện |
|-------|-----------|------|-------|------------------|
| EP-FR09-01 | | | | |

### Bước 5 — Constraints

| C-ID | Ràng buộc | Loại |
|------|-----------|------|
| C-FR09-01 | Chỉ áp mã khi CẢ 5 điều kiện C1–C5 đồng thời thỏa | ràng buộc chéo |
| C-FR09-02 | Số tiền giảm/thành tiền phải đúng công thức F1/F2 (đối chiếu tính tay) | tính toán |

### Bước 6 — Test Cases (Domain)

<!-- Nhớ: mỗi điều kiện C1–C5 có >= 1 TC làm nó fail riêng lẻ; >= 2 TC kiểm chứng công thức
     (percent + fixed) — mã được CHẤP NHẬN chưa chắc TÍNH ĐÚNG TIỀN. -->

| TC-ID | Mô tả | Input / thao tác trên UI (Checkout) | Expected (SRS) | EP/Constraint | Actual | Pass/Fail | Bug-ID |
|-------|-------|-------------------------------------|----------------|---------------|--------|-----------|--------|
| FR09-DT-01 | | | | | | | |

### Tóm tắt coverage

- Số EP: — Số TC: — EP chưa cover:

---

## 2. Boundary Value Analysis — FR-09

### Bước 1 — Boundaries từ SRS

| B-ID | Biến | Ràng buộc | Min | Max | Kiểu |
|------|------|-----------|-----|-----|------|
| B-FR09-01 | `total_amount` (SAVE10) | `>= min_order_amount` | 300.000 | — | numeric |
| B-FR09-02 | `total_amount` (BIGBUY) | `>= min_order_amount` | 500.000 | — | numeric |
| B-FR09-03 | lượt dùng (VIP100) | `< max_uses_per_user` | — | 2 | count |
| B-FR09-04 | hạn dùng | trước `expired_at` | — | `expired_at` | date |

### Bước 2 — BVA Points

<!-- SAVE10: 299.999 (min−1) / 300.000 (ON-POINT — đừng bỏ!) / 300.001 (min+1).
     VIP100: lượt 1 / lượt 2 (on tại max) / lượt 3 (vượt). EXPIRED vs coupon còn hạn. -->

| Point | Biến | Giá trị | Quan hệ biên |
|-------|------|---------|--------------|
| P-01 | | | |

### Bước 3 — Test Cases (BVA)

| TC-ID | Input / thao tác trên UI | Boundary | Expected (SRS) | Actual | Pass/Fail | Bug-ID |
|-------|--------------------------|----------|----------------|--------|-----------|--------|
| FR09-BV-01 | | | | | | |

### Bước 4 — Robust / Edge (bổ sung)

| TC-ID | Input | Ghi chú |
|-------|-------|---------|
| FR09-BV-R01 | mã rỗng `""` | |
| FR09-BV-R02 | mã chữ thường `save10` | UI có ép hoa không? |

### Tóm tắt

- Số boundary: — Số TC: — Fail:

---

## 3. Test Execution — FR-09

**Ngày thực thi:** [.....] · **Chuẩn bị:** product mồi giá [liệt kê] tạo bằng admin UI · **Reset DB** giữa các vòng đếm lượt.

| TC-ID | Mô tả | Expected (SRS) | Actual (quan sát trên UI) | Result | Bug-ID |
|-------|-------|----------------|---------------------------|--------|--------|
| | | | | | |

### Metrics — FR-09

| Designed | Executed | Pass | Fail | Not run | Bugs |
|----------|----------|------|------|---------|------|
| | | | | | |

---

## 4. AI Gap Analysis — FR-09

### 1. Kịch bản/lỗi AI bỏ sót (AI Gaps)

<!-- Gợi ý: AI test < min và > min nhưng QUÊN on-point = min (đúng chỗ bug off-by-one);
     AI tin "giảm 10%" theo spec, không kiểm chứng công thức thật → bỏ lỡ bug tính tiền;
     AI ít khi nghĩ đến việc quan sát số "Tiết kiệm"/"Thành tiền" hiển thị so với tính tay. -->

| Kịch bản / lỗi bị bỏ sót | Root cause | Bài học & khắc phục |
|--------------------------|------------|---------------------|
| | | |

### 2. Cải tiến prompt

1. ...

---
---

# FEATURE C: FR-15 — QUẢN LÝ SẢN PHẨM (PRODUCT CRUD)

## 1. Domain Testing — FR-15

**SUT:** Web Admin (`frontend-admin`, tab Sản phẩm) + `POST/PUT/DELETE /api/products` (`backend/server.js:167–196`) + bảng `products`

### Bước 1 — Phạm vi và SUT

#### Tài liệu đặc tả (SRS FR-15 + FR-12/SEC-03)

| # | Rule | Trích spec |
|---|---|---|
| R1 | Tên sản phẩm: bắt buộc, tối đa 255 ký tự | |
| R2 | Giá: bắt buộc, số dương (> 0) | |
| R3 | Danh mục: bắt buộc, chọn từ danh sách có sẵn | |
| R4 | Sửa 1 sản phẩm chỉ thay đổi sản phẩm đó | |
| R5 | POST/PUT/DELETE /api/products yêu cầu token + role admin (FR-12, SEC-03) | |

#### Phân tích sơ bộ từ mã nguồn thực tế (Code vs SRS)

<!-- Dán handler POST/PUT/DELETE /api/products + form admin UI cho skill spec-vs-code.
     Chú ý: 3 route này có middleware authenticateToken không? có validate name/price/category không?
     Admin UI có chặn user thường đăng nhập không (kiểm tra role ở client)? -->

| Rule | Code thực tế (file:dòng) | Khớp spec? |
|---|---|---|
| R1 | | |
| R2 | | |
| R3 | | |
| R4 | | |
| R5 | | |

### Bước 2 — Input Variables

| ID | Biến | Kiểu | Nguồn | Ràng buộc SRS |
|----|------|------|-------|---------------|
| V1 | `name` | String | Form admin | Bắt buộc, ≤ 255 ký tự |
| V2 | `price` | Integer | Form admin | Bắt buộc, > 0 |
| V3 | `description` | String | Form admin | — |
| V4 | `imageUrl` | String | Form admin | — |
| V5 | `category_id` | Integer (FK) | Dropdown admin | Chọn từ danh sách có sẵn |
| V6 | trạng thái auth (biến ẩn) | State | Đăng nhập admin app | Phải là admin (R5) |

### Bước 3 — Domains

| Biến | Valid Domain | Invalid Domain | Special |
|------|--------------|----------------|---------|
| | | | |

### Bước 4 — Equivalence Partitions

<!-- Nhớ partition cho V6 auth: {admin / user thường / chưa đăng nhập} — test từ UI bằng cách
     đăng nhập admin app với test@eshop.com. Name: rỗng/whitespace/Unicode tiếng Việt/ký tự đặc biệt/255/256. -->

| EP-ID | Loại vùng | Biến | Mô tả | Giá trị đại diện |
|-------|-----------|------|-------|------------------|
| EP-FR15-01 | | | | |

### Bước 5 — Constraints

| C-ID | Ràng buộc | Loại |
|------|-----------|------|
| C-FR15-01 | Chỉ admin được CRUD sản phẩm | access control |
| C-FR15-02 | Sửa sản phẩm X không ảnh hưởng sản phẩm khác | dependency |

### Bước 6 — Test Cases (Domain)

*Phủ cả 3 thao tác Create (trọng tâm) / Update / Delete.*

| TC-ID | Mô tả | Input / thao tác trên UI (Admin) | Expected (SRS) | EP/Constraint | Actual | Pass/Fail | Bug-ID |
|-------|-------|----------------------------------|----------------|---------------|--------|-----------|--------|
| FR15-DT-01 | | | | | | | |

### Tóm tắt coverage

- Số EP: — Số TC: — EP chưa cover:

---

## 2. Boundary Value Analysis — FR-15

### Bước 1 — Boundaries từ SRS

| B-ID | Biến | Ràng buộc | Min | Max | Kiểu |
|------|------|-----------|-----|-----|------|
| B-FR15-01 | `price` | > 0 | 1 | 2147483647 (INTEGER) | numeric |
| B-FR15-02 | `name.length` | bắt buộc, ≤ 255 | 1 | 255 | length |

### Bước 2 — BVA Points

<!-- price: 0 (off) / 1 (on) / giá thường / 2147483647 (on max) / 2147483648 (off max, tràn?) / -1 (robust)
     name: 0 ký tự (off) / 1 (on) / 255 (on max) / 256 (off max) — chuẩn bị chuỗi dài bằng AI/script, paste vào form -->

| Point | Biến | Giá trị | Quan hệ biên |
|-------|------|---------|--------------|
| P-01 | | | |

### Bước 3 — Test Cases (BVA)

| TC-ID | Input / thao tác trên UI | Boundary | Expected (SRS) | Actual | Pass/Fail | Bug-ID |
|-------|--------------------------|----------|----------------|--------|-----------|--------|
| FR15-BV-01 | | | | | | |

### Bước 4 — Robust / Edge (bổ sung)

| TC-ID | Input | Ghi chú |
|-------|-------|---------|
| FR15-BV-R01 | price = -1 | |
| FR15-BV-R02 | price = chữ `"abc"` | ô input có chặn không? |

### Tóm tắt

- Số boundary: — Số TC: — Fail:

---

## 3. Test Execution — FR-15

**Ngày thực thi:** [.....] · **Tài khoản:** `admin@eshop.com` (luồng chuẩn) và `test@eshop.com` (kiểm tra access control) · Reset DB sau các TC tạo rác.

| TC-ID | Mô tả | Expected (SRS) | Actual (quan sát trên UI) | Result | Bug-ID |
|-------|-------|----------------|---------------------------|--------|--------|
| | | | | | |

### Metrics — FR-15

| Designed | Executed | Pass | Fail | Not run | Bugs |
|----------|----------|------|------|---------|------|
| | | | | | |

---

## 4. AI Gap Analysis — FR-15

### 1. Kịch bản/lỗi AI bỏ sót (AI Gaps)

<!-- Gợi ý: AI GIẢ ĐỊNH API/UI đã có auth theo best practice → không sinh TC "user thường CRUD";
     AI chỉ tập trung Create, lơ Update/Delete; AI không nghĩ đến 255/256 hay tràn INTEGER nếu không bị ép. -->

| Kịch bản / lỗi bị bỏ sót | Root cause | Bài học & khắc phục |
|--------------------------|------------|---------------------|
| | | |

### 2. Cải tiến prompt

1. ...

---
---

# FEATURE D: FR-02 TRÊN MOBILE — CROSS-PLATFORM CHECK

> Cùng FR-02 với Feature A (giảng viên cho phép trùng FR giữa các Pool — lưu bằng chứng xác nhận).
> **Không lặp lại** phân tích của Feature A — chương này chỉ gồm: khác biệt tầng mobile client,
> cross-check 2 bug backend qua client thứ 2, và phần riêng mobile.

## 1. Domain Testing — FR-02 Mobile

**SUT:** App Expo (`frontend-mobile/App.js` — `handleLogin` dòng ~186–207, màn hình `renderLogin`) + cùng backend `/api/login`

### Bước 1 — Phạm vi và SUT

#### Hành vi riêng tầng mobile client (Code vs SRS)

> Đã xác minh code: `App.js:204–206` — `catch` nuốt `data.error`, luôn hiện "Đăng nhập thất bại. Vui lòng kiểm tra lại."
> Web client cũng có pattern y hệt (`Login.jsx:18`) ⇒ đây là **lỗi lặp lại trên cả 2 client**, Feature D
> có giá trị chứng minh điều đó bằng thực thi (cross-platform confirmation).

| Khía cạnh | Code mobile (App.js:dòng) | Khớp spec? |
|---|---|---|
| Hiển thị lỗi 401 (sai mật khẩu) | | |
| Hiển thị lỗi 403 (đang bị khóa) | | |
| UI phân biệt được 2 trường hợp? | | |

**Checklist chuẩn bị mobile:** [ ] Expo Go cùng Wi-Fi · [ ] `API_URL = http://192.168.1.12:3000/api` (đã sửa) · [ ] reset DB trước mỗi vòng · [ ] mở song song DevTools trên web để lấy response JSON gốc làm đối chiếu.

### Bước 2 — Input Variables (thừa hưởng Feature A + 1 biến mới)

| ID | Biến | Kiểu | Nguồn | Ràng buộc SRS |
|----|------|------|-------|---------------|
| V1–V4 | như Feature A | | | tái sử dụng, không lặp lại |
| V5 | `error_display` (chỉ có nghĩa trên client) | State | Text hiển thị trên app | Thông báo bị khóa phải khác thông báo sai mật khẩu |

### Bước 4 — Equivalence Partitions (chỉ phần bổ sung)

| EP-ID | Loại vùng | Biến | Mô tả | Giá trị đại diện |
|-------|-----------|------|-------|------------------|
| EP-FR02M-01 | | V5 | | |

### Bước 6 — Test Cases (cross-check + mobile-specific)

| TC-ID | Loại | Thao tác trên app | Expected (SRS) | Actual | Pass/Fail | Bug-ID |
|-------|------|-------------------|----------------|--------|-----------|--------|
| FR02M-DT-01 | cross-check | | | | | |
| FR02M-DT-02 | mobile-specific | | | | | |

---

## 2. Boundary Value Analysis — FR-02 Mobile

Chạy lại **đúng bảng biên của Feature A** (số lần sai 2/3/4; thời gian 29s/30s/31s) nhưng thao tác trên màn hình Login của app → mục tiêu **cross-check**: hành vi biên có tái lập y hệt qua client khác không. Sai lệch giữa 2 lần đo tự nó là 1 finding.

### Test Cases (BVA cross-check)

| TC-ID | Thao tác trên app | Boundary | Expected (SRS) | Kết quả Feature A (web) | Actual (mobile) | Pass/Fail | Bug-ID |
|-------|-------------------|----------|----------------|--------------------------|-----------------|-----------|--------|
| FR02M-BV-01 | | | | | | | |

**Biên bổ sung riêng mobile:** thời điểm text thông báo đổi từ "sai mật khẩu" sang "bị khóa" — kỳ vọng đổi tại đúng lần sai chạm ngưỡng; dự đoán thực tế: **không bao giờ đổi** (1 câu chung).

---

## 3. Test Execution — FR-02 Mobile

**Thiết bị:** [model / emulator], Expo Go [phiên bản] · **Đối chiếu:** DevTools Network trên web song song.

| TC-ID | Expected (SRS) | Actual trên app (nguyên văn) | Response backend (đối chiếu DevTools) | Result | Bug-ID |
|-------|----------------|------------------------------|----------------------------------------|--------|--------|
| | | | | | |

### Bảng cross-check với Feature A

| Hành vi | Web (Feature A) | Mobile | Nhất quán? |
|---|---|---|---|
| Số lần sai đến khi khóa | | | |
| Thời gian khóa đo được | | | |
| Thông báo khi bị khóa | | | |

### Metrics — FR-02 Mobile

| Designed | Executed | Pass | Fail | Not run | Bugs |
|----------|----------|------|------|---------|------|
| | | | | | |

---

## 4. AI Gap Analysis — FR-02 Mobile

<!-- Gợi ý — loại gap KHÁC với A/B/C, tốt cho AI Critique: AI mặc định app hiển thị đúng data.error
     (pattern RN phổ biến) → không phát hiện catch nuốt lỗi trừ khi được dán đúng đoạn handleLogin.
     Đây là gap "đọc code hời hợt" vs gap "suy diễn thiếu dữ kiện" ở feature khác. -->

| Kịch bản / lỗi bị bỏ sót | Root cause | Bài học & khắc phục |
|--------------------------|------------|---------------------|
| | | |

### 2. Cải tiến prompt

1. ...

---
---

# TỔNG KẾT

## Bảng tổng hợp

| Feature | Designed | Executed | Pass | Fail | Not run | Bugs |
|---------|----------|----------|------|------|---------|------|
| FR-02 (web) | | | | | | |
| FR-09 | | | | | | |
| FR-15 | | | | | | |
| FR-02 (mobile) | | | | | | |
| **Total** | | | | | | |

## Các bug quan trọng nhất

1. ...

## Kết luận

[2–4 câu: các nhóm lỗi chính so với SRS, tầng nào (backend logic / client hiển thị / GUI), bằng chứng ở đâu]
