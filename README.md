# HW02 — Domain Testing on EShop

**Sinh viên:** Phạm Vũ Ngọc Duy — **MSSV:** 23127183
**SUT:** EShop — https://github.com/ttbhanh/eshop-sut
**Kỹ thuật:** Domain Testing + Boundary Value Analysis
**Repo GitHub Issues (nhóm — chính thức):** https://github.com/Kurumi2324/SoftwareTestingHW2_Group10/issues
**Repo GitHub cá nhân:** https://github.com/DuyPham111/HW02 (tài khoản GitHub: DuyPham111)

---

## 1. Tính năng đã chọn (mỗi pool 1)

| Pool | FR | Tính năng | Nền tảng test |
|---|---|---|---|
| A | FR-02 | Đăng nhập & Khóa tài khoản | Web (`frontend-web`, trang Đăng nhập) |
| B | FR-09 | Mã giảm giá (Coupon) | Web (`frontend-web`, trang Checkout) |
| C | FR-15 | Quản lý Sản phẩm (CRUD) | Web Admin (`frontend-admin`) |
| D | FR-02 | Đăng nhập & Khóa tài khoản (Mobile) | Mobile (Expo, `frontend-mobile`) |

## 2. Nội dung bài nộp

| File / thư mục | Nội dung |
|---|---|
| `reports/Main_Report.md` / `.pdf` | Báo cáo chính: 4 chương feature — Domain Testing (6 bước) + BVA (4 bước) + Execution + AI Gap |
| `reports/Bug_Report.md` / `.pdf` | Bug hợp nhất: 1 block/bug + ảnh + link GitHub Issues |
| `reports/AI_Audit_Report.md` / `.pdf` | AI Audit Report (bắt buộc) |
| `reports/AI_Critique.md` / `.pdf` | AI Critique 200–300 từ (bắt buộc) |
| `reports/git_commit_log.txt` | Git log (1 step = 1 commit) |
| `reports/FR-XX_bugs/` | Ảnh/video bằng chứng bug theo feature |
| `skills/` | 3 Agent Skills (SKILL.md) + link video demo |

## 3. Test Summary Report

| Feature | Kỹ thuật | Thiết kế | Thực thi | Pass | Fail | Chưa thực thi |
|---|---|:--:|:--:|:--:|:--:|:--:|
| A (FR-02) | Domain | 10 | 10 | 6 | 4 | 0 |
| A (FR-02) | BVA | 11 | 11 | 7 | 4 | 0 |
| B (FR-09) | Domain | 10 | 10 | 6 | 4 | 0 |
| B (FR-09) | BVA | 10 | 10 | 7 | 3 | 0 |
| C (FR-15) | Domain | 8 | 8 | 3 | 5 | 0 |
| C (FR-15) | BVA | 7 | 7 | 5 | 2 | 0 |
| D (FR-02 Mobile) | Domain | 6 | 6 | 2 | 4 | 0 |
| D (FR-02 Mobile) | BVA | 4 | 4 | 2 | 2 | 0 |
| **Tổng** | | **66** | **66** | **38** | **28** | **0** |

**Số bug theo mức độ:**

| Feature | Critical | High/Major | Medium | Low/Minor | Tổng |
|---|:--:|:--:|:--:|:--:|:--:|
| A (FR-02) | 0 | 1 | 3 | 2 | 6 |
| B (FR-09) | 2 | 0 | 2 | 0 | 4 |
| C (FR-15) | 1 | 1 | 1 | 1 | 4 |
| D (FR-02 Mobile) | 0 | 0 | 1 | 1 | 2 (+ B001, B002 tái lập từ Feature A) |
| **Tổng** | **3** | **2** | **7** | **4** | **16 bug ID duy nhất** |

## 4. Demo video (Agent Skill)

| Video | Nội dung | Link |
|---|---|---|
| Video 1 | Demo skill end-to-end trên FR-09 (spec-vs-code → domain → BVA → chạy UI thấy bug) | [Xem trên YouTube](https://www.youtube.com/watch?v=qCm-Q3DnU4g) |

## 5. Self-Assessment

| No. | Criteria | Grade | Self-Assessed |
|---|---|:--:|:--:|
| 1 | Feature A (Domain + Boundary) | 25 | 25 |
| 2 | Feature B (Domain + Boundary) | 25 | 25 |
| 3 | Feature C (Domain + Boundary) | 25 | 25 |
| 4 | Feature D (Mobile, Domain + Boundary) | 15 | 15 |
| 5 | Agent Skills | 10 | 10 |
| | **Total** | **100** | **100** |

**Tên file nộp:** `23127183_HW02_AI_DomainTesting_100.zip`
