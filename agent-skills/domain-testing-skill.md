# Agent Skill — Domain Testing

**Mục đích:** áp dụng kỹ thuật Domain Testing (equivalence partitioning) lên 1 feature bất kỳ của SUT, theo đúng trình tự được dạy — AI là trợ lý có kỷ luật, không phải black box.

**Cách dùng:** dán prompt dưới đây vào AI (Claude/ChatGPT/...), thay các phần `[...]`. Chạy TUẦN TỰ từng bước — không gộp; review output mỗi bước trước khi sang bước sau.

---

## Bước 1 — Xác định input domain

```
Bạn là test analyst. Đây là spec của feature [FR-XX — tên]:
[dán spec]
Đây là code xử lý thực tế:
[dán code handler + code UI liên quan]

Liệt kê TOÀN BỘ biến input của feature, gồm cả BIẾN ẨN (trạng thái hệ thống: đăng nhập,
khóa, số lượt đã dùng...). Với mỗi biến: kiểu dữ liệu, miền hợp lệ, miền không hợp lệ.
Chỉ dựa trên spec + code được dán — không suy diễn rule không tồn tại.
```

## Bước 2 — Chia equivalence partitions

```
Với các biến trên, chia equivalence partitions. Yêu cầu:
- Mỗi biến có cả partition valid và invalid.
- Biến chuỗi phải có: rỗng, whitespace-only, Unicode/tiếng Việt, ký tự đặc biệt.
- Không có 2 partition trùng nghĩa.
Format: | Biến | Partition ID | Mô tả | Giá trị đại diện | Valid? |
Giá trị đại diện phải dùng được ngay với dữ liệu seed: [dán seed liên quan].
```

## Bước 3 — Sinh domain test cases

```
Sinh test case Domain theo nguyên tắc: mỗi TC phủ đúng 1 partition invalid,
các biến khác giữ giá trị đại diện valid. Thêm 1–2 TC all-valid làm baseline.
Mọi TC phải THAO TÁC ĐƯỢC TỪ UI [tên trang/màn hình].
Expected phải xác định (số cụ thể / message cụ thể của SUT), theo SPEC chứ không theo code.
Format: | ID | Partition phủ | Thao tác trên UI | Expected | Ghi chú |. ID: FRxx-DT-01...
```

## Bước 4 — Tự review (con người làm, không phải AI)

- Đối chiếu từng partition với checklist mục 5.1 của roadmap.
- Loại TC trùng/vô nghĩa; sửa Expected mơ hồ; bổ sung partition AI sót.
- Ghi Human Review Notes + log phiên vào `ai-audit-report.md`.
