# Feature D — FR-02 Mobile — Boundary Value Analysis

## 1. Mục tiêu

Chạy lại **đúng bảng biên của Feature A** (số lần sai 2/3/4; thời gian khóa 29s/30s/31s) nhưng thao tác trên màn hình Login của app mobile → **kiểm chứng chéo (cross-check)**: 2 hành vi biên của backend có tái lập y hệt qua client khác không. Sai lệch giữa 2 lần đo (web vs mobile) tự nó là 1 finding.

## 2. Biên bổ sung riêng cho mobile

<!-- Biên "thời điểm đổi nội dung thông báo": kỳ vọng message đổi từ "sai mật khẩu"
     sang "bị khóa" tại đúng lần sai chạm ngưỡng. Quan sát trên app: có đổi không? -->

| Biến | Biên | Kỳ vọng theo spec |
|---|---|---|
| Lần sai mà message trên app đổi nội dung | tại ngưỡng khóa | app hiển thị thông báo bị khóa khác với sai mật khẩu |

## 3. BVA Test Cases

| ID | Biên | Thao tác trên app | Expected (theo spec) | Đối chiếu web/Feature A | Ghi chú |
|---|---|---|---|---|---|
| FR02M-BVA-01 | | | | | |

## 4. Trình tự áp dụng

1. Reset DB → thao tác trên app → ghi text hiển thị nguyên văn → đối chiếu response gốc.
2. ...

## Review checklist

- [ ] Đủ bộ biên số-lần-sai và thời-gian-khóa như Feature A
- [ ] Có cột đối chiếu kết quả với Feature A (web)
- [ ] Có biên riêng mobile về nội dung thông báo

<!-- Human Review Notes: -->
