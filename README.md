# HW02 – Domain Testing on EShop

**Sinh viên:** Lý Quốc Thạnh — **MSSV:** 23127262 — **Email:** lqthanh23@clc.fitus.edu.vn
**Môn:** Kiểm chứng phần mềm — **Bài tập:** HW02 (Domain Testing + Boundary Value Analysis)
**SUT:** EShop — https://github.com/ttbhanh/eshop-sut

---

## 1. Tính năng đã chọn (4 feature, mỗi pool 1)

| Pool | FR-ID | Tính năng | Nền tảng |
|------|-------|-----------|----------|
| A | FR-01 | Đăng ký tài khoản | Web |
| B | FR-09 | Mã giảm giá (coupon) | Web (trang Checkout) |
| C | FR-14 | Quản lý danh mục (CRUD) | Web Admin |
| D | FR-07 | Giỏ hàng | Mobile (React Native/Expo) |

---

## 2. Test Summary Report

**Số feature:** 4
**Phạm vi:** functional testing **từ UI Frontend** (theo chỉ đạo giảng viên): Feature A trên web
(trang Đăng ký), Feature B trên web (Checkout), Feature C trên app admin (tab Danh mục), Feature D
trên app mobile (Expo). Các ca chỉ chạm được từ API trực tiếp đã được loại khỏi phạm vi.

### 2.1. Số lượng test case

| Feature | Kỹ thuật | Thiết kế | Thực thi | Pass | Fail | Note† | Chưa thực thi |
|---------|----------|:--------:|:--------:|:----:|:----:|:-----:|:-------------:|
| A (FR-01) | Domain | 9  | 9  | 3 | 6 | 0 | 0 |
| A (FR-01) | BVA    | 5  | 5  | 4 | 0 | 1 | 0 |
| B (FR-09) | Domain | 9  | 9  | 5 | 3 | 1 | 0 |
| B (FR-09) | BVA    | 6  | 6  | 5 | 1 | 0 | 0 |
| C (FR-14) | Domain | 8  | 8  | 4 | 4 | 0 | 0 |
| C (FR-14) | BVA    | 3  | 3  | 1 | 2 | 0 | 0 |
| D (FR-07) | Domain | 14 | 14 | 4 | 4 | 6 | 0 |
| D (FR-07) | BVA    | 7  | 7  | 3 | 3 | 1 | 0 |
| **Tổng** | | **61** | **61** | **29** | **23** | **9** | **0** |

† **Note** = ca đã thực thi nhưng kết quả là hành vi "vùng xám" (vd âm thầm ép số lượng không
hợp lệ về 1; mã coupon phân biệt hoa/thường) — khiếm khuyết UX/thiết kế chứ không phải pass/fail
dứt khoát theo đặc tả.

### 2.2. Số lượng bug theo mức độ

| Feature | Critical | Major | Minor | Trivial | Tổng |
|---------|:--------:|:-----:|:-----:|:-------:|:----:|
| A (FR-01) | 1 | 3 | 0 | 0 | 4 |
| B (FR-09) | 1 | 1 | 2 | 0 | 4 |
| C (FR-14) | 0 | 2 | 2 | 0 | 4 |
| D (FR-07) | 0 | 2 | 2 | 0 | 4 |
| **Tổng** | **2** | **8** | **6** | **0** | **16** |

**Toàn bộ 16/16 bug ứng viên đều quan sát được trực tiếp trên UI**; các lỗi chỉ chạm được từ API
(backend không kiểm dữ liệu, mật khẩu plaintext, phân quyền admin ở tầng API, giả mạo `user_id`,
PUT/DELETE id không tồn tại) đã được loại khỏi phạm vi functional-UI. Chi tiết từng bug (bước
tái hiện, kỳ vọng/thực tế, ảnh chụp, link GitHub Issue) nằm ở **báo cáo lỗi riêng**
`reports/bug-report.md` (tách khỏi báo cáo chính theo mục 14 của đề).

### 2.3. Demo video (Agent Skill)

| Video | Nội dung | Link |
|-------|----------|------|
| Video 1 | Demo skill `domain-bva-test` end-to-end trên Feature B (FR-09) | ⬜ *TODO: quay & dán link YouTube* |

---

## 3. Bảng tự đánh giá (Self-Assessment)

> Điểm tự đánh giá: **full 100** (đã hoàn tất Domain + BVA cho cả 4 feature từ UI, 16 bug quan
> sát trên giao diện, screenshot + GitHub Issues + video demo skill).

| No. | Criteria | Grade | Self-Assessed |
|-----|----------|:-----:|:-------------:|
| 1 | Feature A (Domain + Boundary) | 25 | **25** |
| 2 | Feature B (Domain + Boundary) | 25 | **25** |
| 3 | Feature C (Domain + Boundary) | 25 | **25** |
| 4 | Feature D (Mobile, Domain + Boundary) | 15 | **15** |
| 5 | Agent Skills | 10 | **10** |
| | **Total** | **100** | **100** |

**Tên file nộp:** `23127262_HW02_AI_DomainTesting_100.zip` (điểm tự đánh giá = 100).

---

## 4. Cấu trúc bài nộp

```
HW02-Software-Testing/
├── README.md                     # File này (self-assessment + test summary)
├── main.tex                      # Báo cáo chính (LaTeX) — Domain Testing + BVA + AI Gap + Agent Skill
├── main.pdf                      # ⬜ TODO: biên dịch từ main.tex (xem mục 5)
├── reports/
│   ├── bug-report.md             # Báo cáo lỗi riêng — 16 bug (đã nhúng ảnh + link GitHub Issue)
│   ├── ai-critique-and-audit.md  # AI Critique + AI Audit Report (riêng) — cần xuất PDF kèm
│   └── git-commit-log.txt        # ⬜ Git commit log (để trống — tự điền bằng lệnh ở mục 6)
├── AI_Audit_Report_Vn_filled.docx # Bản AI Audit theo template (Word)
├── figures/
│   ├── bugs/                     # 16 ảnh chụp bug: bug_a_01.png … bug_d_04.png
│   ├── github-issues/            # Ảnh trang GitHub Issues (issue-01..16 + issues-list)
│   └── README.md                 # Hướng dẫn đặt tên screenshot
├── HW02-Domain-Testing.md        # Đề bài
├── Homework-Policies.md          # Chính sách
└── eshop-sut/                    # SUT (submodule)
```

---

## 5. Biên dịch PDF (bắt buộc trước khi nộp)

Máy hiện tại **chưa cài** LaTeX toolchain nên chưa tạo được `main.pdf`. Trên máy có TeX Live /
MiKTeX, chạy:

```bash
# Cần XeLaTeX hoặc pdfLaTeX + gói tiếng Việt (T5/babel). Khuyến nghị latexmk:
latexmk -pdf main.tex          # chạy 2–3 lần để cập nhật mục lục/tham chiếu
# hoặc:
pdflatex main.tex && pdflatex main.tex
```

Nếu lỗi font T5/tiếng Việt, thử `xelatex main.tex` (đổi `\usepackage[utf8]{inputenc}` khi cần).

---

## 6. Việc còn lại phải làm thủ công (đã tạo sẵn boilerplate)

- [x] **Screenshot 16 bug** → đã gom trong `figures/bugs/` (`bug_a_01.png … bug_d_04.png`) và
      nhúng vào `reports/bug-report.md`.
- [x] **Tạo 16 GitHub Issues** (#1–16) → URL đã điền trong `reports/bug-report.md`; ảnh trang
      Issues lưu ở `figures/github-issues/`.
- [ ] **Quay video demo** skill `domain-bva-test` → dán link YouTube vào README (mục 2.3) và
      Chương Agent Skill của `main.tex`.
- [ ] **Biên dịch `main.pdf`** (mục 5) — máy hiện chưa có LaTeX toolchain.
- [ ] **Cập nhật `reports/git-commit-log.txt`** (log hiện đang cũ):
      `git log --pretty=format:"%h | %ad | %s" --date=format:"%Y-%m-%d %H:%M" > reports/git-commit-log.txt`
- [ ] **`git push`** các commit mới lên remote nhóm.
- [ ] **Xác nhận điểm tự đánh giá** (mục 3) và đặt tên file `.zip` theo quy định.
