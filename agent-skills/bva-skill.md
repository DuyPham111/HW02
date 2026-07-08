# Agent Skill — Boundary Value Analysis (3-point)

**Mục đích:** sinh bộ test case BVA đầy đủ on/off/in-point cho mọi biến có miền số/thứ tự/ngày của 1 feature.

**Cách dùng:** chạy sau khi đã có domain model (skill Domain Testing). Dán prompt, thay `[...]`.

---

## Bước 1 — Nhận diện biến có biên

```
Từ spec và domain model sau: [dán]
Liệt kê mọi biến có miền SỐ / THỨ TỰ / NGÀY kèm biên min/max và NGUỒN của biên
(câu spec nào, hằng số nào trong seed/code). Đừng bỏ biến ẩn có biên
(số lần thử, số lượt dùng, thời gian khóa).
```

## Bước 2 — Sinh bảng 3-point

```
Với từng biến + biên ở trên, sinh bảng BVA 3-point:
off(min−1), on(min), in(giữa), on(max), off(max+1).
BẮT BUỘC có on-point (giá trị ĐÚNG BẰNG biên) — không được bỏ.
Thêm biên đặc biệt khi áp dụng: 0, số âm, rỗng, tràn INTEGER (2147483647/2147483648),
biên ngày (hôm qua / hôm nay / ngày mai).
```

## Bước 3 — Chuyển thành test cases

```
Mỗi giá trị biên → 1 TC thao tác được từ UI [tên trang]. Nếu giá trị khó tạo từ UI,
đề xuất cách chuẩn bị dữ liệu (vd: tạo product mồi giá đúng biên bằng admin UI).
Format: | ID | Biên | Giá trị | Thao tác trên UI | Expected theo spec |. ID: FRxx-BVA-01...
```

## Bước 4 — Tự review (con người)

- Checklist 5.2 roadmap: đủ min−1/min/min+1/max−1/max/max+1? On-point có mặt?
- Ghi Human Review Notes + log audit.
