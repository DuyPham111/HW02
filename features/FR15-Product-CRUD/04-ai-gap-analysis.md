# Feature C — FR-15 — AI Gap Analysis

## 1. AI đã bỏ sót gì

<!-- Gap kinh điển ở FR15:
     - AI GIẢ ĐỊNH API đã được bảo vệ bằng auth/role (theo best practice) nên không sinh
       TC "user thường thao tác CRUD" — trong khi đây là bug nặng nhất của feature.
     - AI có thể chỉ test Create mà lơ Update/Delete.
     - AI ít khi nghĩ đến biên tràn INTEGER hay name 255/256 nếu không bị ép. -->

| # | AI bỏ sót / sai | Phát hiện nhờ đâu |
|---|---|---|
| 1 | | |

## 2. Vì sao AI sót (root cause)

| # | Nguyên nhân | Loại (prompt / giới hạn AI / độ phức tạp) |
|---|---|---|
| 1 | | |

## 3. Cải thiện prompt

- Prompt cũ: [trích]
- Prompt cải thiện: [vd: "liệt kê biến ẩn gồm trạng thái xác thực; KHÔNG giả định API có auth — kiểm tra trong code"]

<!-- Human Review Notes: -->
