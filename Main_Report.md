# HW02 — Main Report: Domain Testing & Boundary Value Analysis on EShop

**Sinh viên:** [HỌ TÊN] — **MSSV:** [MSSV]
**Ngày thực hiện:** [từ ngày] → [đến ngày]

> File này là phần mở đầu của báo cáo chính. Khi đóng gói, ghép với 16 file trong `features/`
> thành `Main_Report_Full.md` → xuất PDF (xem guide.md mục 7).

## 1. Phạm vi

| Pool | Feature | Nơi test | Handler backend liên quan |
|---|---|---|---|
| A | FR-02 Đăng nhập & Khóa tài khoản | Trang Đăng nhập web (:5173) | `POST /api/login` |
| B | FR-09 Mã giảm giá | Trang Checkout web (:5173) | `POST /api/apply-coupon` |
| C | FR-15 Quản lý Sản phẩm | Web Admin (:5174), tab Sản phẩm | `POST/PUT/DELETE /api/products` |
| D | FR-02 trên Mobile | App Expo, màn hình Login | `POST /api/login` (cùng handler Pool A) |

**Phạm vi thực thi:** functional testing bắt đầu tương tác từ web/mobile UI (theo xác nhận của giảng viên ngày 2026-07-08). Không có test case gọi API trực tiếp; DevTools chỉ dùng quan sát response làm bằng chứng đối chiếu khi cần.

## 2. Môi trường

- Backend API: `http://localhost:3000` — Node.js + Express + SQLite
- Frontend Web: `http://localhost:5173` · Web Admin: `http://localhost:5174` · Mobile: Expo Go, `API_URL = http://[IP-LAN]:3000/api`
- Tài khoản seed: `admin@eshop.com / Admin123!` · `test@eshop.com / Test1234!`
- Reset DB giữa các vòng test có trạng thái: `node database.js`

## 3. Quy trình áp dụng (chung cho 4 feature)

1. **Business Analysis:** đọc spec (`README.md` FR-XX) và code handler; lập bảng *rule kỳ vọng vs hành vi thực tế*, đánh dấu điểm nghi vấn.
2. **Domain Testing:** xác định biến input (kể cả biến ẩn như trạng thái khóa/quyền); chia equivalence partitions valid/invalid; sinh TC theo nguyên tắc mỗi TC chỉ 1 partition invalid.
3. **BVA:** xác định biến có biên từ spec; sinh bộ 3-point (off/on/in) cho từng biên, bắt buộc có on-point.
4. **Thực thi:** chạy toàn bộ TC trên UI, ghi Actual nguyên văn, chụp screenshot mọi TC Fail.
5. **Bug reporting:** gộp TC fail cùng nguyên nhân thành bug, đăng GitHub Issues kèm ảnh.
6. **AI Gap Analysis:** đối chiếu output AI ban đầu với kết quả cuối, phân tích nguyên nhân sót.

Mỗi bước là 1 commit riêng (xem `git-commit-log.txt`). Mỗi phiên AI được ghi trong `ai-audit-report.md`.

## 4. Tổng hợp TC Fail đã xác nhận

<!-- Điền sau khi thực thi xong cả 4 feature — mỗi dòng 1 TC fail -->

| TC ID | Feature | Mô tả fail | Bug | Screenshot |
|---|---|---|---|---|
| | | | | |

## 5. Báo cáo chi tiết từng feature

Nội dung Domain Testing / BVA / Execution / Gap Analysis của từng feature nằm ở các chương tiếp theo (ghép từ `features/`):

- Feature A — FR-02: `features/FR02-Login-Lockout/`
- Feature B — FR-09: `features/FR09-Discount-Coupons/`
- Feature C — FR-15: `features/FR15-Product-CRUD/`
- Feature D — FR-02 Mobile: `features/FR02-Mobile-Login-Lockout/`

---
