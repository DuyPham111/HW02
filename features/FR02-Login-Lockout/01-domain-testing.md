# Feature A — FR-02 Đăng nhập & Khóa tài khoản — Domain Testing

**Nơi test:** trang Đăng nhập, `frontend-web` (http://localhost:5173)
**Handler backend:** `POST /api/login` (`backend/server.js`)
**Spec:** `eshop-sut/README.md` mục FR-02

## 1. Business Analysis — Spec vs Code

### 1.1 Rule kỳ vọng (theo spec)

| # | Rule | Trích spec |
|---|---|---|
| R1 | Mỗi lần sai tăng bộ đếm đúng 1 đơn vị | |
| R2 | Sai ≥ 3 lần liên tiếp → khóa 30 giây | |
| R3 | Thông báo lỗi không lộ nguyên nhân (email sai vs password sai giống nhau) | |
| R4 | Đăng nhập đúng → reset bộ đếm, trả JWT | |

### 1.2 Hành vi thực tế trong code

<!-- Điền từ output AI (prompt Bước 1 trong guide) SAU KHI đã tự kiểm chứng bằng cách chạy app -->

| Rule | Code thực tế (file:dòng) | Khớp spec? |
|---|---|---|
| R1 | | |
| R2 | | |
| R3 | | |
| R4 | | |

### 1.3 Điểm nghi vấn (candidate bug — sẽ xác nhận ở bước thực thi)

- [ ] ...
- [ ] ...

## 2. Input Domain

| Biến | Kiểu | Miền hợp lệ | Miền không hợp lệ | Ghi chú |
|---|---|---|---|---|
| email | string | | | |
| password | string | | | |
| account_state (biến ẩn) | state | | | trạng thái khóa lưu trong DB |

## 3. Domain Model (Equivalence Partitions)

<!-- Gợi ý số partition: email 4 (đã đăng ký/chưa đăng ký/sai định dạng/rỗng),
     password 3 (đúng/sai/rỗng), state 2 (active/locked). Giá trị đại diện hợp lệ:
     test@eshop.com / Test1234! -->

| Biến | Partition ID | Mô tả | Giá trị đại diện | Valid? |
|---|---|---|---|---|
| email | E1 | | | |
| email | E2 | | | |
| password | P1 | | | |
| state | S1 | | | |

## 4. Domain Test Cases

Nguyên tắc: mỗi TC phủ đúng 1 partition invalid, các biến còn lại giữ đại diện valid.

| ID | Partition phủ | Thao tác trên UI | Expected (theo spec) | Ghi chú |
|---|---|---|---|---|
| FR02-DT-01 | E1+P1+S1 (all valid) | | | |
| FR02-DT-02 | | | | |
| FR02-DT-03 | | | | |

## 5. Trình tự áp dụng kỹ thuật

1. ...
2. ...

## Review checklist (tick trước khi commit)

- [ ] Mọi biến đều có partition valid + invalid
- [ ] Có partition rỗng cho biến chuỗi
- [ ] Có biến ẩn account_state
- [ ] Mỗi invalid partition test riêng lẻ
- [ ] Expected xác định, đúng message tiếng Việt của SUT

<!-- Human Review Notes: [ghi bạn đã sửa gì so với output AI — bắt buộc] -->
