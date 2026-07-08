# Feature D — FR-02 Đăng nhập & Khóa tài khoản trên MOBILE — Domain Testing

**Nơi test:** app Expo (`frontend-mobile/App.js`, màn hình Login — hàm `handleLogin`)
**Handler backend:** `POST /api/login` — **cùng handler với Feature A**
**Tham chiếu:** phân tích spec-vs-code đầy đủ ở `../FR02-Login-Lockout/01-domain-testing.md` — KHÔNG lặp lại, chỉ nêu phần khác biệt ở tầng mobile.

## 1. Business Analysis — phần riêng của mobile client

### 1.1 Rule kỳ vọng (spec áp dụng chung mọi client)

| # | Rule | Trích spec |
|---|---|---|
| R1–R4 | (như Feature A) | xem Feature A |
| R5 | Hệ thống trả thông báo lỗi phù hợp — người dùng phải biết tài khoản đang bị khóa (403) khác với sai mật khẩu (401) | |

### 1.2 Hành vi thực tế ở tầng mobile (`handleLogin`, App.js)

<!-- Đọc đoạn try/catch của handleLogin: điều gì xảy ra với data.error mà backend trả về? -->

| Khía cạnh | Code thực tế (App.js:dòng) | Khớp spec? |
|---|---|---|
| Hiển thị lỗi 401 (sai mật khẩu) | | |
| Hiển thị lỗi 403 (bị khóa) | | |
| Phân biệt được 2 trường hợp trên UI? | | |

### 1.3 Điểm nghi vấn (candidate bug — mobile-specific)

- [ ] ...

## 2. Input Domain (thừa hưởng Feature A + 1 chiều mới)

| Biến | Partition | Ghi chú |
|---|---|---|
| email, password, account_state | (như Feature A) | tái sử dụng |
| error_display (biến mới, chỉ có nghĩa trên mobile) | {lỗi do 401 / lỗi do 403} | kiểm tra UI có phân biệt không |

## 3. Domain Test Cases (chỉ phần bổ sung/cross-check)

<!-- Không copy nguyên bộ TC của Feature A. Chọn: (1) các TC đại diện để cross-check
     hành vi backend tái lập trên mobile; (2) TC mới cho error_display. -->

| ID | Loại | Thao tác trên app | Expected (theo spec) | Ghi chú |
|---|---|---|---|---|
| FR02M-DT-01 | cross-check | | | |
| FR02M-DT-02 | mobile-specific | | | |

## 4. Trình tự áp dụng kỹ thuật

1. ...

## Review checklist

- [ ] Không copy nguyên văn Feature A — trình bày dạng cross-check + phần riêng mobile
- [ ] Có TC kiểm tra text hiển thị khi 401 vs khi 403 (đối chiếu message gốc backend)
- [ ] Ghi cách đối chiếu message gốc (web song song / DevTools network)

<!-- Human Review Notes: -->
