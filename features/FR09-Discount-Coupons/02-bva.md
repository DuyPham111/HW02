# Feature B — FR-09 Mã giảm giá — Boundary Value Analysis

## 1. Biến có biên

<!-- 3 nhóm biên chính:
     (1) total_amount quanh min_order_amount — SAVE10: 299.999 / 300.000 / 300.001
         (và/hoặc BIGBUY: 499.999 / 500.000 / 500.001)
     (2) hạn dùng — coupon EXPIRED (đã hết hạn) vs coupon còn hạn
     (3) số lượt dùng — VIP100 max 2: lượt 1 / lượt 2 / lượt 3
     ĐỪNG QUÊN ON-POINT (= min) — đây là biên hay bị AI bỏ. -->

| Biến | Biên (theo spec) | Nguồn |
|---|---|---|
| total_amount (SAVE10) | min = 300.000 | seed + README FR-09 |
| total_amount (BIGBUY) | min = 500.000 | seed |
| Lượt dùng (VIP100) | max = 2 | seed |
| Hạn dùng | `expired_at` | seed EXPIRED |

## 2. Bảng giá trị biên (3-point)

| Biến | off (min−1) | on (min) | in (min+1) | Ghi chú |
|---|---|---|---|---|
| total (SAVE10) | 299.999 → ? | 300.000 → ? | 300.001 → ? | |
| Lượt VIP100 | lượt 1 → ? | lượt 2 → ? | lượt 3 → ? | on tại max |

## 3. BVA Test Cases

| ID | Biên | Giá trị / thao tác trên UI | Expected (theo spec) | Ghi chú |
|---|---|---|---|---|
| FR09-BVA-01 | | | | |
| FR09-BVA-02 | | | | |

## 4. Trình tự áp dụng

1. ...

## Review checklist

- [ ] On-point `= min` được test tường minh cho ít nhất 1 coupon
- [ ] Biên lượt dùng có đủ trước/tại/vượt max
- [ ] Có ghi cách chuẩn bị dữ liệu (product mồi, reset DB)

<!-- Human Review Notes: -->
