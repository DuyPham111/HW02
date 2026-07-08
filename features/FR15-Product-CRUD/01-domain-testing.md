# Feature C — FR-15 Quản lý Sản phẩm (CRUD) — Domain Testing

**Nơi test:** Web Admin, `frontend-admin` (http://localhost:5174), tab Sản phẩm
**Handler backend:** `POST/PUT/DELETE /api/products` (`backend/server.js`)
**Spec:** `eshop-sut/README.md` mục FR-15 + FR-12/SEC-03 (yêu cầu quyền admin)

## 1. Business Analysis — Spec vs Code

### 1.1 Rule kỳ vọng (theo spec)

| # | Rule | Trích spec |
|---|---|---|
| R1 | Tên sản phẩm: bắt buộc, tối đa 255 ký tự | |
| R2 | Giá: bắt buộc, số dương (> 0) | |
| R3 | Danh mục: bắt buộc, chọn từ danh sách có sẵn | |
| R4 | Sửa 1 sản phẩm chỉ thay đổi sản phẩm đó | |
| R5 | POST/PUT/DELETE /api/products yêu cầu token + role admin (FR-12, SEC-03) | |

### 1.2 Hành vi thực tế trong code

| Rule | Code thực tế (file:dòng) | Khớp spec? |
|---|---|---|
| R1 | | |
| R2 | | |
| R3 | | |
| R4 | | |
| R5 | | |

### 1.3 Điểm nghi vấn (candidate bug)

- [ ] ...

## 2. Input Domain

<!-- Nhớ BIẾN ẨN quan trọng nhất của feature này: trạng thái auth
     {chưa đăng nhập / đăng nhập user thường (test@eshop.com) / đăng nhập admin}.
     Test từ UI: đăng nhập frontend-admin bằng test@eshop.com rồi thử CRUD. -->

| Biến | Kiểu | Miền hợp lệ | Miền không hợp lệ | Ghi chú |
|---|---|---|---|---|
| name | string | | | |
| price | số | | | schema INTEGER |
| description | string | | | |
| imageUrl | string | | | |
| category_id | số (FK) | | | |
| auth_state (biến ẩn) | state | | | admin / user / chưa đăng nhập |

## 3. Domain Model (Equivalence Partitions)

| Biến | Partition ID | Mô tả | Giá trị đại diện | Valid? |
|---|---|---|---|---|
| | | | | |

## 4. Domain Test Cases

<!-- Phủ cả 3 thao tác: Create (trọng tâm), Update, Delete. -->

| ID | Partition phủ | Thao tác trên UI (Admin) | Expected (theo spec) | Ghi chú |
|---|---|---|---|---|
| FR15-DT-01 | all valid (create) | | | |
| FR15-DT-02 | | | | |

## 5. Trình tự áp dụng kỹ thuật

1. ...

## Review checklist

- [ ] Có partition name rỗng / whitespace / rất dài / Unicode-tiếng Việt / ký tự đặc biệt
- [ ] Có partition price 0 / âm / chữ / rất lớn
- [ ] Có partition auth (user thường thao tác CRUD) — nơi bug access control lộ ra
- [ ] Có TC cho category_id không hợp lệ
- [ ] Mỗi invalid partition test riêng lẻ

<!-- Human Review Notes: -->
