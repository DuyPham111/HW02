---
name: hw02-domain-testing
description: >-
  Áp dụng kỹ thuật Domain Testing (equivalence partitioning) cho một feature
  của EShop theo quy trình 6 bước, UI-first. Dùng khi cần phân tích miền đầu
  vào, chia phân vùng tương đương, hoặc thiết kế domain test case cho FR-01..FR-20.
---

# Domain Testing — EShop (quy trình 6 bước, UI-first)

## Nguyên tắc

- Domain Testing = xác định **miền đầu vào** → **phân vùng tương đương** → **ràng buộc chéo** → chọn TC phủ hết vùng + ràng buộc. Phải trình bày từng bước, không chỉ liệt kê TC.
- **UI-first:** mọi TC phải thao tác được từ giao diện (web/mobile). DevTools chỉ để quan sát response làm bằng chứng.
- **Được phép sinh test case cụ thể** (giảng viên cho phép): AI điền sẵn giá trị input cụ thể, các bước thao tác UI, và Expected — để sinh viên chạy ngay và chụp ảnh. Nhưng **Actual/Pass-Fail để trống**, sinh viên tự chạy điền.
- Expected LUÔN theo **spec (SRS)**; Actual chỉ điền sau khi thực thi thật.
- Sau mỗi bước: con người review, sửa, ghi log vào `AI_Audit_Report.md`, rồi mới commit.

## Quy trình 6 bước (ghi vào Main_Report.md, chương FEATURE tương ứng)

### Bước 1 — Phạm vi và SUT
Prompt cho AI:
```
Đây là spec FR-XX: [dán mục FR-XX từ README.md]
Đây là code liên quan: [dán handler backend + component UI, kèm số dòng]
1) Tách spec thành các rule nguyên tử (R1, R2...).
2) Với từng rule: chỉ ra code hiện thực (file:dòng), kết luận KHỚP / KHÁC SPEC / KHÔNG HIỆN THỰC.
3) Với mỗi điểm KHÁC SPEC: mô tả hệ quả NGƯỜI DÙNG THẤY ĐƯỢC trên UI.
Không suy diễn rule không có trong dữ liệu được dán.
```
→ Con người kiểm chứng từng kết luận bằng cách chạy app trước khi tick.

### Bước 2 — Input Variables
Bảng `| ID | Biến | Kiểu | Nguồn | Ràng buộc SRS |`. Bắt buộc liệt kê **biến ẩn** (trạng thái DB: bộ đếm, khóa, lượt dùng, quyền).

### Bước 3 — Domains
Bảng `| Biến | Valid | Invalid | Special |`. Special bắt buộc cân nhắc: rỗng, whitespace, Unicode/tiếng Việt, ký tự đặc biệt, SQLi/XSS payload.

### Bước 4 — Equivalence Partitions
Bảng `| EP-ID | Loại vùng | Biến | Mô tả | Giá trị đại diện |`. EP-ID dạng `EP-FRXX-01`. Giá trị đại diện phải dùng được ngay với seed data.

### Bước 5 — Constraints
Bảng `| C-ID | Ràng buộc | Loại |` (dependency / business rule / GUI rule / access control). Đây là nơi ghi các rule chéo nhiều biến (vd: cả 5 điều kiện coupon đồng thời; reset bộ đếm khi login đúng).

### Bước 6 — Test Cases
Prompt cho AI:
```
Từ EP và Constraints trên, sinh domain test case theo nguyên tắc single-fault:
mỗi TC phủ đúng 1 EP invalid, biến khác giữ đại diện valid. Thêm 1-2 TC all-valid.
Mỗi constraint có >= 1 TC vi phạm + 1 TC thỏa. Mọi TC thao tác từ UI [tên trang].
Format: | TC-ID | Mô tả | Input/thao tác UI | Expected (SRS) | EP/Constraint |
TC-ID dạng FRXX-DT-01. Expected phải xác định (số/message cụ thể), không mơ hồ.
```
Cột `Actual | Pass/Fail | Bug-ID` để trống — chỉ điền khi thực thi.

### Kết bước — Tóm tắt coverage
Số EP / số TC / EP chưa cover (phải = 0).

## Sau khi thiết kế

1. Chạy checklist review: đủ partition rỗng/whitespace/Unicode? có biến ẩn? Expected xác định?
2. Ghi AI gap nếu phát hiện AI sót partition → mục AI Gap Analysis.
3. Commit: `[FRxx][domain] Add domain model and domain test cases`
