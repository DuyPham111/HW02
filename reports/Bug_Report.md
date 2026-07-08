# Consolidated Bug Report — HW02 Domain Testing on EShop

**Họ và tên:** [HỌ TÊN] · **MSSV:** [MSSV] · **Nhóm:** [Nhóm]
**SUT:** EShop — trang Đăng nhập web, Checkout web, Web Admin, App mobile
**GitHub Issues:** https://github.com/DuyPham111/HW02/issues
**Tổng:** [N] bug ([x] Critical, [x] Major, [x] Minor)

> Quy ước: mỗi bug 1 block theo mẫu dưới; ảnh bằng chứng lưu `reports/FR-XX_bugs/BUG-FRXX-00N.png`.
> Khi tạo GitHub Issue: Title theo dòng **Title** trong block, dán Body, **kéo-thả ảnh trực tiếp
> vào issue** (GitHub tự sinh URL ảnh — tránh lỗi ảnh không render), rồi điền Link Issue lại vào block.

---

# FEATURE A: FR-02 — ĐĂNG NHẬP & KHÓA TÀI KHOẢN ([n] bugs)

### BUG-FR02-001: [Tên ngắn gọn — vd: Bộ đếm đăng nhập sai tăng 2 đơn vị, tài khoản bị khóa sau 2 lần]

- **Độ nghiêm trọng (Severity):** [Critical/Major/Minor] — *vì:* [lý do]
- **Độ ưu tiên (Priority):** [High/Medium/Low] — *vì:* [lý do]
- **Thành phần (Component):** [Backend logic / Client UI / GUI]
- **Test Case liên quan:** FR02-DT-xx, FR02-BV-xx
- **Liên quan SRS:** [trích nguyên văn câu spec bị vi phạm]

#### Các bước tái hiện (trên UI):
1. Reset DB: `node database.js` (nếu cần trạng thái sạch)
2. Mở http://localhost:5173/login
3. ...

#### Kết quả mong đợi (Expected):
- [theo SRS]

#### Kết quả thực tế (Actual):
- [quan sát nguyên văn trên UI + số liệu đo được]

#### Bằng chứng (Evidence):
- ![BUG-FR02-001](./FR-02_bugs/BUG-FR02-001.png)

#### GitHub Issue:
- **Title:** `[FR-02] [BUG-FR02-001] <English/VN short title>`
- **Link:** [URL issue]

---

### BUG-FR02-002: [...]

<!-- copy block trên -->

---

# FEATURE B: FR-09 — MÃ GIẢM GIÁ ([n] bugs)

### BUG-FR09-001: [...]

<!-- copy block chuẩn; ảnh lưu ./FR-09_bugs/ -->

---

# FEATURE C: FR-15 — QUẢN LÝ SẢN PHẨM ([n] bugs)

### BUG-FR15-001: [...]

<!-- copy block chuẩn; ảnh lưu ./FR-15_bugs/ -->

---

# FEATURE D: FR-02 MOBILE ([n] bugs)

> Bug backend tái hiện lại trên mobile: **không tạo issue trùng** — comment "Reproduced on mobile client"
> + ảnh mobile vào issue gốc của Feature A, và ghi chú tham chiếu tại đây.
> Chỉ tạo block + issue mới cho bug riêng phát hiện qua mobile.

### BUG-FR02M-001: [...]

<!-- copy block chuẩn; ảnh lưu ./FR-02-mobile_bugs/ -->
