# Feature B — FR-09 Mã giảm giá — Domain Testing

**Nơi test:** trang Checkout, `frontend-web` (http://localhost:5173)
**Handler backend:** `POST /api/apply-coupon` (`backend/server.js`)
**Spec:** `eshop-sut/README.md` mục FR-09 (5 điều kiện C1–C5 + công thức giảm giá)

## 1. Business Analysis — Spec vs Code

### 1.1 Rule kỳ vọng (theo spec)

| # | Rule | Trích spec |
|---|---|---|
| C1 | Mã tồn tại và `is_active = 1` | |
| C2 | Ngày hiện tại trước `expired_at` | |
| C3 | Tổng đơn **>=** `min_order_amount` | |
| C4 | Đã đăng nhập (JWT hợp lệ) | |
| C5 | Số lần đã dùng < `max_uses_per_user` | |
| F1 | percent: `discount = total × value / 100` | |
| F2 | fixed: `discount = value`; `final = total − discount` | |

### 1.2 Hành vi thực tế trong code

| Rule | Code thực tế (file:dòng) | Khớp spec? |
|---|---|---|
| C1 | | |
| C2 | | |
| C3 | | |
| C4 | | |
| C5 | | |
| F1 | | |
| F2 | | |

### 1.3 Điểm nghi vấn (candidate bug)

- [ ] ...

## 2. Input Domain

<!-- Coupon seed dùng làm test data:
     SAVE10 (percent 10, min 300.000) · BIGBUY (fixed 50k, min 500.000)
     VIP100 (fixed 100k, min 300.000, max 2 lượt) · EXPIRED (percent 20, hết hạn 2020-01-01)
     Mẹo tạo tổng đơn đúng biên: tạo product mồi giá 299.999/300.000/300.001 bằng admin UI. -->

| Biến | Kiểu | Miền hợp lệ | Miền không hợp lệ | Ghi chú |
|---|---|---|---|---|
| code | string | | | |
| total_amount (tổng giỏ) | số | | | điều khiển qua product mồi |
| trạng thái đăng nhập (biến ẩn) | state | | | |
| số lượt đã dùng (biến ẩn) | số | | | lưu trong DB — reset được |
| hạn dùng (thuộc tính coupon) | date | | | |

## 3. Domain Model (Equivalence Partitions)

| Biến | Partition ID | Mô tả | Giá trị đại diện | Valid? |
|---|---|---|---|---|
| | | | | |

## 4. Domain Test Cases

| ID | Partition phủ | Thao tác trên UI (Checkout) | Expected (theo spec) | Ghi chú |
|---|---|---|---|---|
| FR09-DT-01 | all valid (percent) | | | |
| FR09-DT-02 | all valid (fixed) | | | |
| FR09-DT-03 | | | | |

## 5. Trình tự áp dụng kỹ thuật

1. ...

## Review checklist

- [ ] Mỗi điều kiện C1–C5 có ≥ 1 TC làm nó fail riêng lẻ
- [ ] Có TC kiểm chứng CÔNG THỨC (số tiền giảm/thành tiền so với tính tay) — chấp nhận mã ≠ tính đúng tiền
- [ ] Có partition mã rỗng / mã không tồn tại / mã EXPIRED
- [ ] Expected xác định theo message tiếng Việt của SUT

<!-- Human Review Notes: -->
