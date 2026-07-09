# Bug Report — HW02 Domain Testing on EShop

**Họ và tên:** [HỌ TÊN] · **MSSV:** [MSSV] · **Nhóm:** [Nhóm]
**Repo GitHub Issues (chứa screenshot):** https://github.com/DuyPham111/HW02/issues
**Môi trường chung:** Windows 11, Chrome [phiên bản], Node v22, backend :3000, web :5173, admin :5174, mobile Expo Go.

> **Cách dùng:** mỗi lỗi = 1 dòng trong bảng dưới + 1 ảnh trong `reports/FR-XX_bugs/BUG-00N.png`.
> Tạo GitHub Issue: Title = cột "Defect Title", body = copy cột "Description", **kéo-thả ảnh trực tiếp
> vào issue** (GitHub tự sinh URL — tránh ảnh không render), rồi điền link issue vào cột "GitHub Issue".
> Severity: Critical (mất tiền/bảo mật/hỏng dữ liệu) > High > Medium > Low (hiển thị/UX).

---

## Bảng tổng hợp lỗi (Defect List)

| Defect ID | Defect Title | Description (Pre-conditions / Steps / Expected / Actual) | Feature | Severity | Screenshot | GitHub Issue | Status |
| :---: | :--- | :--- | :---: | :---: | :--- | :--- | :---: |
| **B001** | [FR-02] Bộ đếm sai tăng nhanh hơn quy định, tài khoản khóa sớm hơn 1 lần | **Pre:** DB vừa reset, user `test@eshop.com`.<br>**Steps:** 1. Mở /login, mở DevTools Network. 2. Nhập sai mật khẩu, bấm Sign In. 3. Lặp lại, quan sát status code từng lần.<br>**Expected:** Spec ghi bộ đếm +1/lần và khóa từ lần sai thứ 3 — với cách backend kiểm tra khóa ở đầu mỗi request (dùng trạng thái đã lưu từ lần trước), điều này có nghĩa HTTP 403 chỉ nên xuất hiện từ **lần thứ 4** trở đi.<br>**Actual:** Status đo được qua 3 lần thử liên tiếp: lần 1 = 401, lần 2 = 401, lần 3 = **403** (đã khóa) — sớm hơn 1 lần so với đúng thiết kế. Khớp với dòng code `login_attempts + 2` (`server.js:54`) thay vì `+ 1`. | FR-02 | `FR-02_bugs/B001.png` | [https://github.com/DuyPham111/HW02/issues/1] | Open |
| **B002** | [FR-02] Thời gian khóa thực tế ~3 phút thay vì 30 giây | **Pre:** Tài khoản đang bị khóa (HTTP 403 xác nhận).<br>**Steps:** 1. Ghi giờ hiện tại. 2. Thử đăng nhập đúng ở các mốc: 30s, >1 phút, >2 phút, >3 phút — quan sát status code mỗi lần.<br>**Expected:** Mở khóa (HTTP 200) sau **30 giây** (spec R2b).<br>**Actual:** 30s → 403 (vẫn khóa); >1 phút → 403; >2 phút (3:59 PM → 4:01 PM) → 403; **sau hơn 3 phút → HTTP 200** (đăng nhập thành công, `me`/`products` tiếp theo đều 200). Khớp chính xác hằng số `Date.now() + 180000` (`server.js:57`). | FR-02 | `FR-02_bugs/B002-start.png`, `FR-02_bugs/B002-endafter3minutes.png` | [https://github.com/DuyPham111/HW02/issues/2] | Open |
| **B003** | [FR-02] UI không phân biệt "sai mật khẩu" và "tài khoản bị khóa" | **Pre:** Tài khoản đang bị khóa (đã xác nhận HTTP 403 ở 2 lần thử trước).<br>**Steps:** 1. Nhập đúng `test@eshop.com` / `Test1234!`. 2. Bấm Sign In. 3. Đọc thông báo. 4. Xem status code trên DevTools Network.<br>**Expected:** Thông báo cho biết tài khoản đang bị khóa (khác câu sai mật khẩu).<br>**Actual:** DevTools xác nhận **HTTP 403** (đang khóa) dù mật khẩu đúng, nhưng UI vẫn hiển thị y hệt "Đăng nhập thất bại. Vui lòng kiểm tra lại." — không có gì phân biệt với trường hợp sai mật khẩu (401). | FR-02 | `FR-02_bugs/B003.png` | [https://github.com/DuyPham111/HW02/issues/3] | Open |
| **B004** | [FR-02] Ô mật khẩu hiển thị rõ ký tự (thiếu `type="password"`) | **Pre:** Ở trang /login.<br>**Steps:** 1. Gõ ký tự vào ô mật khẩu. 2. Quan sát.<br>**Expected:** Hiển thị dạng ẩn `•••` (FR-22).<br>**Actual:** Ký tự hiển thị rõ dạng plaintext (vd `Test1234!`), không bị che. | FR-02 | `FR-02_bugs/B004.png` | [https://github.com/DuyPham111/HW02/issues/4] | Open |
| **B005** | [FR-02] Ô email dùng `type="text"`, không validate định dạng | **Pre:** Ở /login.<br>**Steps:** 1. Nhập email sai định dạng (không có `@`, vd `ghosteshop.com`) và mật khẩu bất kỳ. 2. Bấm Sign In.<br>**Expected:** Bị chặn định dạng email ngay tại form (spec R5, `type="email"`).<br>**Actual:** Không có popup cảnh báo, request vẫn được gửi lên server; hệ thống hiển thị lỗi chung "Đăng nhập thất bại. Vui lòng kiểm tra lại." | FR-02 | `FR-02_bugs/B005.png` | [https://github.com/DuyPham111/HW02/issues/5] | Open |
| **B012** | [FR-02] Trang Đăng nhập hiển thị tiêu đề và nhãn sai ngôn ngữ/ý nghĩa | **Pre:** Ở trang /login.<br>**Steps:** 1. Mở trang /login. 2. Quan sát tiêu đề, nhãn các ô nhập, nhãn nút submit.<br>**Expected:** Tiêu đề "Đăng Nhập"; nhãn "Email"; nút "Đăng Nhập" (tiếng Việt, đúng FR-21 nhất quán ngôn ngữ).<br>**Actual:** Tiêu đề hiển thị "Đăng Ký" (sai — đây là trang Đăng Nhập); nhãn ghi "Username" (tiếng Anh) thay vì "Email"; nút ghi "Sign In" (tiếng Anh) thay vì "Đăng Nhập". | FR-02 | `FR-02_bugs/B012.png` | [https://github.com/DuyPham111/HW02/issues/6] | Open |
| **B006** | [FR-09] Đơn đúng bằng ngưỡng tối thiểu bị từ chối (off-by-one `>`) — lỗi hệ thống, áp dụng cho mọi coupon | **Pre:** User đăng nhập, giỏ = đúng ngưỡng tối thiểu của coupon.<br>**Steps:** 1. Vào Checkout với giỏ = đúng `min_order_amount`. 2. Nhập mã, Áp dụng.<br>**Expected:** Chấp nhận (spec C3 `>=`).<br>**Actual:** Bị từ chối "Đơn hàng chưa đủ giá trị tối thiểu…" ở **CẢ HAI** trường hợp đã test: giỏ = 300.000 + `SAVE10` (min=300.000), và giỏ = 500.000 + `BIGBUY` (min=500.000). Khớp chính xác code `if (total_amount > coupon.min_order_amount)` (`server.js:379`) dùng `>` thay vì `>=` — áp dụng cho MỌI coupon vì cùng 1 dòng code, không riêng SAVE10. So sánh 299.999 (dưới ngưỡng) vs 300.000 (đúng ngưỡng) cho **cùng kết quả từ chối** — chứng minh rõ off-by-one. | FR-09 | `FR-09_bugs/B006-1.png`, `FR-09_bugs/B006-2.png` | [link] | Open |
| **B007** | [FR-09] Công thức percent sai, tiền giảm âm / thành tiền tăng gấp 10 lần | **Pre:** User đăng nhập, giỏ = 350.000 (sản phẩm `TEST-350k`).<br>**Steps:** 1. Vào Checkout. 2. Nhập `SAVE10`, Áp dụng.<br>**Expected:** Giảm 35.000 (10%), còn 315.000 (spec: `discount = total × value / 100`).<br>**Actual:** Hệ thống hiện "Tiết kiệm: **-3.150.000 ₫**" (âm), "Thành tiền: **3.500.000 ₫**" (gấp 10 lần đơn gốc). Khớp chính xác công thức sai trong code `Math.floor(total_amount * (1 - coupon.discount_value))` (`server.js:399-401`) — với `discount_value=10` (10, không phải 0.1), phép tính trở thành `total*(1-10) = -9*total`. | FR-09 | `FR-09_bugs/B007.png` | [link] | Open |
| **B008** | [FR-09] Khách chưa đăng nhập vẫn áp được mã (thiếu kiểm tra C4) | **Pre:** Đã đăng xuất.<br>**Steps:** 1. Vào Checkout với giỏ = 550.000 (`TEST-550k`). 2. Nhập `BIGBUY`, Áp dụng.<br>**Expected:** Từ chối, yêu cầu đăng nhập (spec C4).<br>**Actual:** Mã **được áp dụng thành công** dù chưa đăng nhập — "Áp dụng thành công! Giảm 50.000 ₫", "Thành tiền: 500.000 ₫" — vi phạm C4 (endpoint `/api/apply-coupon` không có middleware `authenticateToken`, chỉ bỏ qua kiểm tra lượt dùng nếu thiếu `user_id`). Khi cố **hoàn tất thanh toán** (bấm "Xác Nhận Thanh Toán") thì mới bị chặn đúng: popup "Lỗi khi thanh toán: Unauthorized" (do `/api/checkout` CÓ `authenticateToken`) — vậy bug chỉ nằm ở bước áp mã, bước thanh toán cuối vẫn được bảo vệ đúng. | FR-09 | `FR-09_bugs/B008.png` | [link] | Open |
| **B009** | [FR-15] Backend không validate giá sản phẩm — chấp nhận giá 0, âm, và rỗng | **Pre:** Đăng nhập admin, tab Sản phẩm.<br>**Steps:** 1. Thêm sản phẩm, name hợp lệ, price=`0`. 2. Lưu. 3. Lặp lại với price=`-1`. 4. Lặp lại với price để trống.<br>**Expected:** Từ chối cả 3 trường hợp (spec R2: giá bắt buộc, phải > 0).<br>**Actual:** Cả 3 trường hợp đều **tạo thành công**: sản phẩm "SP gia 0" hiện giá "0 ₫"; "SP gia am" hiện giá "-1 ₫"; "SP khong gia" hiện giá trống (chỉ ký hiệu "₫", không có số). Khớp với phân tích code: backend không có validate nào cho `price` (`server.js:167–177`); ô price phía client là `type="number"` nhưng **không** có thuộc tính `required` (`App.jsx:502`). | FR-15 | `FR-15_bugs/B009-1.png`, `FR-15_bugs/B009-2.png`, `FR-15_bugs/B009-3.png` | [link] | Open |
| **B010** | [FR-15] Chấp nhận tên sản phẩm chỉ gồm khoảng trắng | **Pre:** Đăng nhập admin, tab Sản phẩm.<br>**Steps:** 1. Thêm sản phẩm, name=`"   "` (3 dấu cách), price hợp lệ. 2. Lưu.<br>**Expected:** Từ chối (tên bắt buộc, spec R1).<br>**Actual:** **Tạo thành công** — sản phẩm hiện trong danh sách với tên hiển thị trống rỗng (chỉ khoảng trắng, không nhìn thấy chữ). Form admin có `required` trên ô name (`App.jsx:498`) nhưng thuộc tính này chỉ chặn chuỗi rỗng tuyệt đối `""`, không chặn chuỗi chỉ gồm khoảng trắng; backend cũng không có validate bổ sung. | FR-15 | `FR-15_bugs/B010.png` | [link] | Open |
| **B011** | [FR-02 Mobile] App không phân biệt sai mật khẩu vs bị khóa | **Pre:** Chạy app Expo, tài khoản đang bị khóa.<br>**Steps:** 1. Thử đăng nhập trên app. 2. Đọc thông báo. 3. Đối chiếu 403 backend.<br>**Expected:** App cho biết đang bị khóa.<br>**Actual:** [điền — dự đoán: cùng câu chung như sai mật khẩu] | FR-02 Mobile | `FR-02-mobile_bugs/B011.png` | [link] | Open |
| **B013** | [FR-09] Ô "Tổng tiền thanh toán" chỉnh sửa tự do — kết hợp với bug công thức coupon (B007) tạo ra chênh lệch tài chính khổng lồ | **Pre:** Đăng nhập, giỏ = 350.000 (`TEST-350k`).<br>**Steps:** 1. Vào Checkout. 2. Sửa ô "Tổng tiền thanh toán" từ `350000` thành `35000000` (thêm 2 số 0). 3. Nhập `SAVE10`, bấm Áp dụng. 4. Bấm "Xác Nhận Thanh Toán". 5. Vào Lịch sử đơn hàng, kiểm tra `Tổng tiền` đơn vừa tạo.<br>**Expected:** Ô tổng tiền phải chỉ đọc, không cho sửa; backend phải tự tính lại tổng tiền từ giỏ hàng thật (350.000), không nhận giá trị client gửi.<br>**Actual:** Ô nhập tự do (`<input type="number">` không `readOnly`, `Checkout.jsx:93-102`) — giá trị này (`editableTotal`) được dùng làm `total_amount` cho CẢ `/api/apply-coupon` (`Checkout.jsx:30`) LẪN `/api/checkout` (`Checkout.jsx:47`). Sau khi sửa thành 35.000.000 và áp `SAVE10`, hệ thống hiện "Tiết kiệm: **-315.000.000 ₫**", "Thành tiền: **350.000.000 ₫**" (do cộng hưởng với bug công thức B007). **Đã xác nhận bằng đơn hàng thật:** đơn `#4` trong Lịch sử đơn hàng lưu **Tổng tiền = 350.000.000 ₫** — từ một giỏ hàng thật chỉ đáng 350.000 ₫, backend chấp nhận và lưu số tiền gấp 1000 lần, xác nhận không hề tự tính lại từ giỏ hàng. | FR-09 | `FR-09_bugs/B013-1.png`, `FR-09_bugs/B013-2.png` | [link] | Open |

> **Thêm dòng khi phát hiện lỗi mới.** Chỉ đưa vào bảng những lỗi bạn **đã tự chạy và chụp được ảnh**.
> Nếu một TC dự đoán ở trên chạy ra KHÔNG phải bug (khác dự đoán) → xóa dòng đó, đừng báo bug sai.

---

## Ảnh minh chứng — FR-02

### B001 — Khóa xuất hiện sớm hơn 1 lần (401 → 401 → 403)
![B001](FR-02_bugs/B001.png)

### B002 — Thời gian khóa thực tế ~3 phút
**Lúc bắt đầu khóa:**
![B002-start](FR-02_bugs/B002-start.png)

**Sau hơn 3 phút, đã mở khóa:**
![B002-end](FR-02_bugs/B002-endafter3minutes.png)

### B003 — Mật khẩu đúng vẫn bị từ chối (403) khi đang khóa, thông báo không phân biệt
![B003](FR-02_bugs/B003.png)

### B004 — Ô mật khẩu hiển thị rõ ký tự
![B004](FR-02_bugs/B004.png)

### B005 — Email sai định dạng không bị chặn
![B005](FR-02_bugs/B005.png)

### B012 — Tiêu đề/nhãn trang Đăng nhập sai ngôn ngữ
![B012](FR-02_bugs/B012.png)

---

## Ảnh minh chứng — FR-09

### B006 — Đơn đúng bằng ngưỡng tối thiểu vẫn bị từ chối (off-by-one, cả SAVE10 và BIGBUY)
![B006-1](FR-09_bugs/B006-1.png)
![B006-2](FR-09_bugs/B006-2.png)

### B007 — Công thức percent sai (tiết kiệm âm, thành tiền tăng gấp 10 lần)
![B007](FR-09_bugs/B007.png)

### B008 — Khách chưa đăng nhập vẫn áp mã thành công
![B008](FR-09_bugs/B008.png)

### B013 — Ô tổng tiền chỉnh sửa tự do, cộng hưởng với B007 tạo đơn hàng giả 350.000.000 ₫
**Lúc áp coupon (đã sửa tổng tiền thành 35.000.000):**
![B013-1](FR-09_bugs/B013-1.png)

**Đơn hàng thật lưu trong Lịch sử đơn hàng:**
![B013-2](FR-09_bugs/B013-2.png)

---

## Ghi chú phạm vi (lỗi ngoài phạm vi UI — KHÔNG tính là bug nộp)

Theo phạm vi Functional UI, các lỗi sau chỉ chạm được bằng gọi API trực tiếp nên **không đưa vào bảng bug**, chỉ ghi nhận ở AI Gap Analysis:

- FR-15: `POST/PUT/DELETE /api/products` thiếu kiểm tra token/role (broken access control) — admin UI đã chặn user thường đăng nhập nên không tái hiện được từ giao diện.
- Mật khẩu lưu plaintext; backend không validate khi gọi API trực tiếp.
