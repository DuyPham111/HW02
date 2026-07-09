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
| **B001** | [FR-02] Bộ đếm sai tăng 2, tài khoản khóa sau 2 lần thay vì 3 | **Pre:** DB vừa reset, user `test@eshop.com`.<br>**Steps:** 1. Mở /login. 2. Nhập sai mật khẩu, bấm Sign In. 3. Lặp lại và đếm số lần đến khi bị khóa.<br>**Expected:** Khóa sau **3** lần sai (spec R2a); mỗi lần +1.<br>**Actual:** [điền — dự đoán: khóa sau 2 lần] | FR-02 | `FR-02_bugs/B001.png` | [link] | Open |
| **B002** | [FR-02] Thời gian khóa ~3 phút thay vì 30 giây | **Pre:** Tài khoản vừa bị khóa.<br>**Steps:** 1. Bấm giờ từ lúc khóa. 2. Thử đăng nhập đúng ở mốc 30s, 1 phút, 3 phút.<br>**Expected:** Mở khóa sau **30 giây** (spec R2b).<br>**Actual:** [điền — dự đoán: ~180 giây] | FR-02 | `FR-02_bugs/B002.png` | [link] | Open |
| **B003** | [FR-02] UI không phân biệt "sai mật khẩu" và "tài khoản bị khóa" | **Pre:** Tài khoản đang bị khóa (đã xác nhận HTTP 403 ở 2 lần thử trước).<br>**Steps:** 1. Nhập đúng `test@eshop.com` / `Test1234!`. 2. Bấm Sign In. 3. Đọc thông báo. 4. Xem status code trên DevTools Network.<br>**Expected:** Thông báo cho biết tài khoản đang bị khóa (khác câu sai mật khẩu).<br>**Actual:** DevTools xác nhận **HTTP 403** (đang khóa) dù mật khẩu đúng, nhưng UI vẫn hiển thị y hệt "Đăng nhập thất bại. Vui lòng kiểm tra lại." — không có gì phân biệt với trường hợp sai mật khẩu (401). | FR-02 | `FR-02_bugs/B003.png` | [link] | Open |
| **B004** | [FR-02] Ô mật khẩu hiển thị rõ ký tự (thiếu `type="password"`) | **Pre:** Ở trang /login.<br>**Steps:** 1. Gõ ký tự vào ô mật khẩu. 2. Quan sát.<br>**Expected:** Hiển thị dạng ẩn `•••` (FR-22).<br>**Actual:** Ký tự hiển thị rõ dạng plaintext (vd `Test1234!`), không bị che. | FR-02 | `FR-02_bugs/B004.png` | [link] | Open |
| **B005** | [FR-02] Ô email dùng `type="text"`, không validate định dạng | **Pre:** Ở /login.<br>**Steps:** 1. Nhập email sai định dạng (không có `@`, vd `ghosteshop.com`) và mật khẩu bất kỳ. 2. Bấm Sign In.<br>**Expected:** Bị chặn định dạng email ngay tại form (spec R5, `type="email"`).<br>**Actual:** Không có popup cảnh báo, request vẫn được gửi lên server; hệ thống hiển thị lỗi chung "Đăng nhập thất bại. Vui lòng kiểm tra lại." | FR-02 | `FR-02_bugs/B005.png` | [link] | Open |
| **B012** | [FR-02] Trang Đăng nhập hiển thị tiêu đề và nhãn sai ngôn ngữ/ý nghĩa | **Pre:** Ở trang /login.<br>**Steps:** 1. Mở trang /login. 2. Quan sát tiêu đề, nhãn các ô nhập, nhãn nút submit.<br>**Expected:** Tiêu đề "Đăng Nhập"; nhãn "Email"; nút "Đăng Nhập" (tiếng Việt, đúng FR-21 nhất quán ngôn ngữ).<br>**Actual:** Tiêu đề hiển thị "Đăng Ký" (sai — đây là trang Đăng Nhập); nhãn ghi "Username" (tiếng Anh) thay vì "Email"; nút ghi "Sign In" (tiếng Anh) thay vì "Đăng Nhập". | FR-02 | `FR-02_bugs/B012.png` | [link] | Open |
| **B006** | [FR-09] Đơn đúng bằng ngưỡng tối thiểu bị từ chối (off-by-one `>`) | **Pre:** User đăng nhập, giỏ = đúng 300.000.<br>**Steps:** 1. Vào Checkout. 2. Nhập `SAVE10`, Áp dụng.<br>**Expected:** Chấp nhận, giảm 30.000, còn 270.000 (spec C3 `>=`).<br>**Actual:** [điền — dự đoán: báo chưa đủ ngưỡng] | FR-09 | `FR-09_bugs/B006.png` | [link] | Open |
| **B007** | [FR-09] Công thức percent sai, tiền giảm âm / thành tiền tăng ~10 lần | **Pre:** User đăng nhập, giỏ = 30.000.000.<br>**Steps:** 1. Vào Checkout. 2. Nhập `SAVE10`, Áp dụng.<br>**Expected:** Giảm 3.000.000, còn 27.000.000.<br>**Actual:** [điền — dự đoán: tiết kiệm âm, thành tiền ~300 triệu] | FR-09 | `FR-09_bugs/B007.png` | [link] | Open |
| **B008** | [FR-09] Khách chưa đăng nhập vẫn áp được mã (thiếu kiểm tra C4) | **Pre:** Đã đăng xuất.<br>**Steps:** 1. Vào Checkout với giỏ ≥ 300.000. 2. Nhập `SAVE10`, Áp dụng.<br>**Expected:** Từ chối, yêu cầu đăng nhập (spec C4).<br>**Actual:** [điền] | FR-09 | `FR-09_bugs/B008.png` | [link] | Open |
| **B009** | [FR-15] Chấp nhận giá = 0 / giá âm (thiếu validate price > 0) | **Pre:** Đăng nhập admin, tab Sản phẩm.<br>**Steps:** 1. Thêm sản phẩm, name hợp lệ, price=`0` (rồi `-1`). 2. Lưu.<br>**Expected:** Từ chối (spec R2 giá > 0).<br>**Actual:** [điền — dự đoán: lưu thành công] | FR-15 | `FR-15_bugs/B009.png` | [link] | Open |
| **B010** | [FR-15] Chấp nhận tên chỉ gồm khoảng trắng | **Pre:** Đăng nhập admin, tab Sản phẩm.<br>**Steps:** 1. Thêm sản phẩm, name=`"   "`, price hợp lệ. 2. Lưu.<br>**Expected:** Từ chối (tên bắt buộc, R1).<br>**Actual:** [điền] | FR-15 | `FR-15_bugs/B010.png` | [link] | Open |
| **B011** | [FR-02 Mobile] App không phân biệt sai mật khẩu vs bị khóa | **Pre:** Chạy app Expo, tài khoản đang bị khóa.<br>**Steps:** 1. Thử đăng nhập trên app. 2. Đọc thông báo. 3. Đối chiếu 403 backend.<br>**Expected:** App cho biết đang bị khóa.<br>**Actual:** [điền — dự đoán: cùng câu chung như sai mật khẩu] | FR-02 Mobile | `FR-02-mobile_bugs/B011.png` | [link] | Open |

> **Thêm dòng khi phát hiện lỗi mới.** Chỉ đưa vào bảng những lỗi bạn **đã tự chạy và chụp được ảnh**.
> Nếu một TC dự đoán ở trên chạy ra KHÔNG phải bug (khác dự đoán) → xóa dòng đó, đừng báo bug sai.

---

## Ghi chú phạm vi (lỗi ngoài phạm vi UI — KHÔNG tính là bug nộp)

Theo phạm vi Functional UI, các lỗi sau chỉ chạm được bằng gọi API trực tiếp nên **không đưa vào bảng bug**, chỉ ghi nhận ở AI Gap Analysis:

- FR-15: `POST/PUT/DELETE /api/products` thiếu kiểm tra token/role (broken access control) — admin UI đã chặn user thường đăng nhập nên không tái hiện được từ giao diện.
- Mật khẩu lưu plaintext; backend không validate khi gọi API trực tiếp.
