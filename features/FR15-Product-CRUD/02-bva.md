# Feature C — FR-15 Quản lý Sản phẩm — Boundary Value Analysis

## 1. Biến có biên

<!-- 2 nhóm biên chính:
     (1) price quanh 0 (spec: > 0): -1 / 0 / 1 — và biên trên INTEGER: 2147483647 / 2147483648
     (2) độ dài name (spec: bắt buộc, tối đa 255): 0 (rỗng) / 1 / 255 / 256 -->

| Biến | Min | Max | Nguồn |
|---|---|---|---|
| price | > 0 | INTEGER (2^31−1) | README FR-15 + schema |
| name.length | 1 (bắt buộc) | 255 | README FR-15 |

## 2. Bảng giá trị biên (3-point)

| Biến | off (min−1) | on (min) | in | on (max) | off (max+1) |
|---|---|---|---|---|---|
| price | 0 → ? | 1 → ? | 1.000.000 | 2147483647 → ? | 2147483648 → ? |
| name.length | 0 (rỗng) → ? | 1 ký tự → ? | 50 ký tự | 255 → ? | 256 → ? |

<!-- Lưu ý price: spec nói "> 0" nên on-point dưới là 1, off-point là 0; thêm -1 làm điểm invalid sâu. -->

## 3. BVA Test Cases

| ID | Biên | Giá trị / thao tác trên UI | Expected (theo spec) | Ghi chú |
|---|---|---|---|---|
| FR15-BVA-01 | | | | |

## 4. Trình tự áp dụng

1. ...

## Review checklist

- [ ] Biên price có 0, 1, −1, giá trị tràn INTEGER
- [ ] Biên name có 0/1/255/256 ký tự (chuẩn bị chuỗi 255/256 ký tự bằng script/AI)
- [ ] Ghi rõ cách nhập giá trị dài vào form (paste)

<!-- Human Review Notes: -->
