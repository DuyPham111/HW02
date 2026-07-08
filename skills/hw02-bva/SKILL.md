---
name: hw02-bva
description: >-
  Áp dụng Boundary Value Analysis (3-point: off/on/in) cho một feature của
  EShop theo quy trình 4 bước, UI-first. Dùng khi cần xác định biên, sinh BVA
  points và test case biên cho FR-01..FR-20.
---

# Boundary Value Analysis — EShop (quy trình 4 bước, UI-first)

## Nguyên tắc

- Chạy SAU khi đã có domain model (skill `hw02-domain-testing`).
- **On-point là bắt buộc** — giá trị đúng bằng biên là nơi bug off-by-one (`>` vs `>=`) lộ ra.
- Expected theo spec; biên "thật" của implementation chỉ được kết luận sau khi đo/chạy thật.
- UI-first: nếu giá trị biên khó tạo từ UI, chuẩn bị dữ liệu bằng chính UI (vd tạo product mồi giá đúng biên bằng admin UI).

## Quy trình 4 bước (ghi vào Main_Report.md)

### Bước 1 — Boundaries từ SRS
Prompt cho AI:
```
Từ spec và domain model sau: [dán]
Liệt kê mọi biến có miền SỐ / ĐẾM / NGÀY / ĐỘ DÀI kèm min/max và NGUỒN biên
(câu spec nào, hằng số nào trong seed). Đừng bỏ biến ẩn có biên
(số lần thử, số lượt dùng, thời gian khóa).
Format: | B-ID | Biến | Ràng buộc | Min | Max | Kiểu |
```

### Bước 2 — BVA Points
Prompt cho AI:
```
Với từng boundary, sinh điểm 3-point: off(min-1), on(min), in, on(max), off(max+1).
BẮT BUỘC có on-point. Bổ sung biên đặc biệt khi áp dụng: 0, số âm, tràn INTEGER
(2147483647/2147483648), biên ngày (trước hạn / đúng hạn / sau hạn).
Format: | Point | Biến | Giá trị | Quan hệ biên |
```

### Bước 3 — Test Cases
Mỗi point → 1 TC thao tác từ UI, kèm cách chuẩn bị dữ liệu. Format:
`| TC-ID | Input/thao tác UI | Boundary | Expected (SRS) | Actual | Pass/Fail | Bug-ID |` — TC-ID dạng `FRXX-BV-01`. Actual để trống đến khi thực thi.

### Bước 4 — Robust / Edge bổ sung
`FRXX-BV-R01…`: rỗng, whitespace, giá trị âm, chuỗi thay số, input siêu dài.

### Kết bước — Tóm tắt
Số boundary / số TC / Fail.

## Checklist trước khi commit

- [ ] Mỗi biên đủ min−1 / min / min+1 (và max−1 / max / max+1 nếu có max)
- [ ] On-point được test tường minh
- [ ] Ghi rõ cách chuẩn bị dữ liệu + cách reset trạng thái giữa các TC

Commit: `[FRxx][bva] Add boundary value analysis test cases`
