---
name: hw02-spec-vs-code
description: >-
  Đối chiếu từng rule trong spec (SRS) với hiện thực trong mã nguồn để phát
  hiện điểm không tuân thủ (candidate bug) trước khi thiết kế test case. Dùng
  ở bước Business Analysis đầu tiên của mỗi feature EShop.
---

# Spec vs Code Diff — săn candidate bug (bước 1 của mỗi feature)

## Mục đích

Domain Testing/BVA chỉ chứng minh được bug khi so **spec (kỳ vọng)** với **hành vi thật**. Skill này ép AI đối chiếu có kỷ luật, không suy diễn theo best practice.

## Prompt

```
Bạn là reviewer. Nhiệm vụ: đối chiếu SPEC với CODE, KHÔNG suy diễn.

SPEC của [FR-XX]:
[dán nguyên văn mục FR-XX trong README.md]

CODE thực tế:
[dán handler backend + đoạn UI liên quan, kèm số dòng]

Yêu cầu:
1. Tách spec thành danh sách rule nguyên tử (R1, R2, ...), mỗi rule 1 câu.
2. Với TỪNG rule: chỉ ra đoạn code hiện thực (file:dòng) và kết luận
   KHỚP / KHÁC SPEC / KHÔNG HIỆN THỰC — trích biểu thức code làm bằng chứng.
3. Với mỗi điểm KHÁC SPEC: mô tả hệ quả NGƯỜI DÙNG THẤY ĐƯỢC trên UI
   (để thiết kế TC chứng minh từ giao diện).
4. Không kết luận bug khi chưa có bằng chứng trong code được dán.

Format: | Rule | Spec nói | Code làm (file:dòng) | Kết luận | Hệ quả trên UI |
```

## Quy tắc sử dụng (con người)

- Output chỉ là **candidate bug** — phải xác nhận bằng thực thi trên UI trước khi đưa vào Bug_Report.
- Mỗi kết luận KHÁC SPEC → thiết kế ≥ 1 TC (Domain hoặc BVA) nhắm thẳng vào nó.
- Ghi log phiên vào `AI_Audit_Report.md` kèm Human Review Notes (đã kiểm chứng gì, AI sai đâu).
