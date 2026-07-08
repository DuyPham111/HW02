# Agent Skill — Spec vs Code Diff (tìm bug cài sẵn)

**Mục đích:** ép AI đối chiếu từng rule trong spec với hiện thực trong code để lộ ra điểm không tuân thủ — kỹ thuật "săn bug cài sẵn" trước khi thực thi. Đây là skill nâng cao, bổ trợ cho 2 skill kia.

**Cách dùng:** chạy Ở BƯỚC ĐẦU TIÊN của mỗi feature (Business Analysis).

---

## Prompt

```
Bạn là reviewer. Nhiệm vụ: đối chiếu SPEC với CODE, KHÔNG suy diễn.

SPEC của [FR-XX]:
[dán nguyên văn mục FR-XX trong README.md]

CODE thực tế:
[dán handler backend + đoạn UI liên quan, kèm số dòng]

Yêu cầu:
1. Tách spec thành danh sách rule nguyên tử (R1, R2, ...), mỗi rule 1 câu.
2. Với TỪNG rule, chỉ ra đoạn code hiện thực nó (file:dòng) và kết luận:
   KHỚP / KHÁC SPEC / KHÔNG HIỆN THỰC. Trích biểu thức code làm bằng chứng.
3. Với mỗi điểm KHÁC SPEC: mô tả hệ quả quan sát được trên UI
   (người dùng sẽ thấy gì sai) — để thiết kế TC chứng minh.
4. Tuyệt đối không kết luận bug khi chưa có bằng chứng trong code được dán.

Format: | Rule | Spec nói | Code làm (file:dòng) | Kết luận | Hệ quả trên UI |
```

## Quy tắc sử dụng (con người)

- Output chỉ là **candidate bug** — phải xác nhận bằng thực thi thật trước khi đưa vào bug report.
- Mỗi kết luận "KHÁC SPEC" → thiết kế ít nhất 1 TC (Domain hoặc BVA) nhắm thẳng vào nó.
- Ghi log audit sau mỗi lần chạy skill.
