# Hướng dẫn quay video demo skill (HW02)

**Yêu cầu đề bài:** *"Submit the skills together with demonstration videos (YouTube
links) that show, end to end, how you used the skills on a complete feature."*

Nghĩa là: quay **1 video** (hoặc 1 series) cho thấy **toàn bộ quy trình** dùng Agent
Skills để kiểm thử **một feature trọn vẹn** — từ lúc gọi skill sinh test case → review →
thực thi → báo bug → log audit → commit. Không phải chỉ demo "sinh test case rồi thôi".

- **Feature khuyến nghị:** **FR-09 — Mã giảm giá (coupon)** (đúng như README mục 2.3 đã ghi).
  Mã giảm giá được áp ngay trên **trang Checkout của web EShop** (ô "Mã Giảm Giá" → nút "Áp
  dụng" → hiện Tiết kiệm/Thành tiền), nên quay **thao tác trên giao diện** thấy kết quả trực
  quan; lại có đủ Domain + BVA + nhiều bug → minh họa được cả 4 skill.
- **Quan trọng — theo lời giảng viên (forum Q\&A HW02):** *functional testing phải thực hiện
  **từ UI Frontend***. Vì vậy phần "thực thi thật" trong video **quay thao tác trên trình
  duyệt** (web EShop), **không** gọi `curl`/API trực tiếp.
- **Độ dài mục tiêu:** 8–15 phút. Đủ để đi hết quy trình, không lê thê.
- **Nơi dán link:** README mục **2.3** và **Chương 6** của `main.tex`.

---

## 1. Chuẩn bị trước khi quay (checklist)

- [ ] **Phần mềm quay màn hình:**
  - Windows/Mac/Linux: **OBS Studio** (miễn phí) — quay màn hình + mic.
  - Hoặc nhanh gọn: **Windows Game Bar** (`Win+G`), **QuickTime** (Mac), hoặc extension
    Loom/ScreenRec.
- [ ] **Micro** hoạt động, phòng yên tĩnh. (Có thể thuyết minh tiếng Việt.)
- [ ] **Phóng to font** terminal + editor (VS Code: `Ctrl` `+`) để chữ đọc được ở 1080p.
- [ ] **Backend EShop đang chạy** ở `localhost:3000` (web UI gọi tới; không demo bằng curl):
  ```bash
  cd eshop-sut/backend && npm install && npm start
  ```
- [ ] **Web EShop (frontend) đang chạy** — đây là **giao diện để demo thực thi**:
  ```bash
  cd eshop-sut/frontend-web && npm install && npm run dev   # mở http://localhost:5173
  ```
- [ ] **Reset DB seed** nếu cần để coupon `SAVE10`, `BIGBUY`, `VIP100`, `EXPIRED` sạch.
- [ ] Mở sẵn: (1) cửa sổ **Claude Code**, (2) **trình duyệt** mở web EShop (trang Checkout),
  (3) **VS Code** mở `reports/` và `main.tex`, (4) trình duyệt tab **GitHub Issues**.
- [ ] **Xóa/ẩn thông tin nhạy cảm** (token, email cá nhân không cần thiết) khỏi màn hình.
- [ ] Chuẩn bị sẵn **kịch bản prompt** (mục 3) để không ấp úng khi quay.

> Mẹo: quay **thử 1 phút** trước, kiểm tra tiếng + độ nét chữ rồi mới quay thật.

---

## 2. Cấu trúc video (storyboard theo cảnh)

| Cảnh | Thời lượng | Nội dung | Skill minh họa |
|------|:----------:|----------|----------------|
| 0. Intro | 0:00–0:40 | Chào, giới thiệu tên/MSSV, SUT EShop, feature demo = FR-09, và 4 skill sẽ dùng. | — |
| 1. Gọi skill Domain+BVA | 0:40–4:00 | Mở Claude Code, gọi `domain-bva-test` cho FR-09; đi qua **từng bước** (biến → phân hoạch → biên → domain matrix → BVA → test case → gap analysis). Nhấn mạnh: **không** dùng 1 prompt chung chung. | `domain-bva-test` |
| 2. Human review | 4:00–5:30 | Dừng ở 1–2 bước, chỉ ra chỗ AI sai/thiếu, **sửa tay**, giải thích vì sao. (Đây là tiêu chí "Human review".) | Nguyên tắc review |
| 3. Commit per step | 5:30–6:30 | Chạy skill 3: `git add` file bước đó + `git commit` theo Conventional Commits (`test(FR-09): ...`). Cho thấy `git log`. | Skill 3 |
| 4. Thực thi thật (trên UI) | 6:30–9:00 | **Trên trình duyệt web EShop:** thêm sản phẩm vào giỏ → vào Checkout → nhập mã coupon, bấm "Áp dụng", xem Tiết kiệm/Thành tiền; làm vài ca then chốt (mã hợp lệ, mã tại biên `min_order_amount`, mã làm thành tiền sai). Đối chiếu Expected vs Actual ngay trên màn hình; chỉ ra ca **Fail**. | Thực thi + điền cột kết quả |
| 5. Báo bug | 9:00–11:30 | Từ ca Fail → chạy skill 2 (Bug Reporter): tạo block LaTeX trong `bug-report`/`main.tex`, chạy `gh issue create`, kéo-thả screenshot lên GitHub Issue, dán URL. | Skill 2 |
| 6. Log AI Audit | 11:30–13:00 | Chạy skill 1 (AI Audit Logger): thêm entry vào `ai-audit-report` (tool, ngày giờ, prompt, output, human review). | Skill 1 |
| 7. Kết | 13:00–14:00 | Tóm tắt: đã đi hết vòng đời 1 feature bằng skill; mở PDF/README cho thấy kết quả cuối. | — |

> Bạn có thể **gộp** hoặc **tách** thành nhiều video (Video 1: domain-bva-test; Video 2:
> bug + audit + commit). Nếu tách, dán **nhiều dòng link** vào README mục 2.3.

---

## 3. Kịch bản lời thoại + prompt mẫu (đọc theo)

**Cảnh 0 — Intro (nói):**
> "Chào thầy/cô, em là Lý Quốc Thạnh, MSSV 23127262. Video này demo cách em dùng bộ Agent
> Skills để kiểm thử end-to-end tính năng **FR-09 Mã giảm giá** của EShop, gồm 4 skill:
> domain-bva-test, Bug Reporter, AI Audit Logger, và Commit-per-Step."

**Cảnh 1 — gọi skill (gõ trong Claude Code):**
> `Dùng skill domain-bva-test cho FR-09 (mã giảm giá). Thiết kế ca kiểm thử theo hướng thao
> tác trên UI trang Checkout (eshop-sut/frontend-web/src/pages/Checkout.jsx); tham chiếu logic
> apply-coupon ở backend để hiểu hành vi. Đi qua từng bước và dừng cho em review mỗi bước.`

Vừa chạy vừa thuyết minh mỗi bước skill in ra: *"Bước 1 skill liệt kê biến `code`,
`total_amount`, `user_id`… Bước 3 phân hoạch lớp tương đương EC… Bước 6 BVA 3 giá trị quanh
`min_order_amount`…"*

**Cảnh 2 — review (nói + sửa):**
> "Ở đây AI giả định `total_amount > min` dùng dấu `>`; em kiểm lại source thấy đúng là `>`
> (nên đơn = min bị từ chối) — đây là một điểm biên quan trọng, em giữ lại và nhấn mạnh."

**Cảnh 4 — thực thi trên UI (thao tác trình duyệt):**
> Trên web EShop: thêm một sản phẩm vào giỏ → vào **Checkout** → ở ô "Tổng tiền thanh toán"
> đặt đúng bằng `min_order_amount` của mã (ví dụ 300.000) → nhập mã vào ô "Mã Giảm Giá" →
> bấm **Áp dụng**.
> "Đơn đúng bằng min 300k bị **từ chối** ngay trên màn hình vì code dùng `>` thay vì `>=` —
> đây là bug biên, thấy trực tiếp trên giao diện Checkout."

**Cảnh 5 — báo bug (gõ trong Claude Code):**
> `Report bug: FR-09, kỹ thuật BVA, off-by-one ở min_order_amount (dùng > thay vì >=),
> severity Major. Tạo block trong bug-report + gh issue.`

**Cảnh 6 — log audit (gõ trong Claude Code):**
> `Log audit cho phiên domain testing FR-09 vừa rồi.`

---

## 4. Sau khi quay: dựng & upload

1. **Cắt gọt** đoạn thừa (thời gian chờ AI/cài đặt): bất kỳ editor nào — CapCut, DaVinci
   Resolve (free), Clipchamp (Windows), iMovie (Mac). Có thể tua nhanh ×4 đoạn AI đang gõ.
2. **Xuất** 1080p, MP4.
3. **Upload YouTube:**
   - Đặt chế độ **Unlisted (Không công khai)** — thầy/cô có link là xem được, không lộ ra
     ngoài. (Đừng để **Private** vì người khác không mở được link.)
   - Tiêu đề gợi ý: `HW02 Domain Testing — Demo Agent Skills trên FR-09 — 23127262`.
   - Mô tả: dán mốc thời gian các cảnh (timestamp) để dễ chấm.
4. **Copy link** dạng `https://youtu.be/xxxx`.

---

## 5. Dán link vào bài nộp

**a) README.md — mục 2.3** (thay dòng TODO):
```markdown
| Video 1 | Demo skill `domain-bva-test` + Bug Reporter + Audit + Commit end-to-end trên FR-09 | https://youtu.be/xxxx |
```

**b) main.tex — Chương 6** (mục về Agent Skills), thêm:
```latex
\noindent\textbf{Video demo:} \url{https://youtu.be/xxxx}
```

> Sau khi dán link, nhớ **biên dịch lại `main.pdf`** (README mục 5) và cập nhật
> `reports/git-commit-log.txt` nếu có commit mới.

---

## 6. Rubric tự kiểm (đảm bảo "end-to-end")

- [ ] Video cho thấy **gọi skill thật** trong Claude Code (không phải chỉ đọc report).
- [ ] Đi qua **đủ các bước** của kỹ thuật (Domain **và** BVA), không nhảy cóc.
- [ ] Có cảnh **human review + sửa tay** (tiêu chí bắt buộc của đề).
- [ ] Có **thực thi thật trên giao diện** web EShop (thao tác trình duyệt, thấy Expected vs Actual, ít nhất 1 bug) — **không** demo bằng `curl`/API.
- [ ] Có **báo bug** (LaTeX + GitHub Issue + screenshot) và **log audit**.
- [ ] Có **commit theo từng bước** (Conventional Commits).
- [ ] Link YouTube **Unlisted**, mở được ở chế độ ẩn danh, đã dán vào README + main.tex.
