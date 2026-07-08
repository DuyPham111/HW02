# Feature A — FR-02 Đăng nhập & Khóa tài khoản — Boundary Value Analysis

## 1. Biến có biên

<!-- FR02 có 2 biên ordinal/time kinh điển:
     (1) số lần đăng nhập sai liên tiếp — biên spec tại 3;
     (2) thời gian khóa — biên spec tại 30 giây.
     Nhớ: Expected LUÔN ghi theo spec; Actual đo thật ở file 03. -->

| Biến | Biên (theo spec) | Nguồn |
|---|---|---|
| Số lần sai liên tiếp | 3 | README FR-02 |
| Thời gian khóa | 30 giây | README FR-02 |

## 2. Bảng giá trị biên (3-point)

| Biến | off (dưới biên) | on (đúng biên) | off (trên biên) |
|---|---|---|---|
| Số lần sai | 2 lần → ? | 3 lần → ? | 4 lần → ? |
| Thời gian sau khóa | 29s → ? | 30s → ? | 31s → ? |

## 3. BVA Test Cases

| ID | Biên | Thao tác trên UI | Expected (theo spec) | Ghi chú |
|---|---|---|---|---|
| FR02-BVA-01 | | | | |
| FR02-BVA-02 | | | | |
| FR02-BVA-03 | | | | |

## 4. Trình tự áp dụng

1. ...

## Review checklist

- [ ] Có on-point cho cả 2 biên
- [ ] Ghi rõ cách reset trạng thái giữa các TC (`node database.js`)
- [ ] Cách đo thời gian khóa được mô tả (đồng hồ/timestamp)

<!-- Human Review Notes: -->
