# AI Critique & AI Audit Report — HW02 Domain Testing

**Sinh viên:** Lý Quốc Thạnh (23127262)

> Bản Markdown này song song với phần Phụ lục A (AI Critique) và Phụ lục B (AI Audit) trong
> báo cáo chính `main.tex` → PDF.
>
> **Phạm vi:** theo chỉ đạo của giảng viên (forum Q&A HW02), toàn bộ kiểm thử là *functional
> testing từ UI Frontend*. Kết quả cuối: **61 ca kiểm thử / 16 bug** (2 Critical, 8 Major,
> 6 Minor), tất cả quan sát trực tiếp trên giao diện.

---

## Phần 1 — AI Critique (≈270 từ)

Trong suốt bài tập, AI (Claude Code) là một trợ lý mạnh khi **đã được dẫn dắt từng bước**, nhưng
bộc lộ vài điểm mù đáng chú ý. **Thứ nhất, về phạm vi kiểm thử:** khi chưa được nhắc, AI thiết kế
nhiều ca ở *tầng API* (gọi thẳng `POST /api/register` với body rỗng, đọc mật khẩu qua
`/users/me`…) — vượt ra ngoài yêu cầu *functional testing từ UI Frontend* của giảng viên. Con
người phải phân định ca nào *người dùng thao tác được trên giao diện* với ca chỉ chạm tới bằng
request tự chế; AI ban đầu trộn lẫn hai loại.

**Thứ hai**, AI yếu ở các lỗi thuộc *biến đầu ra* và *phép tính thực tế*. Lỗi công thức phần trăm
của coupon (`total × (1 − discount)`) chỉ lộ khi đọc đúng biểu thức số học, không suy được từ tên
biến — may mắn là hệ quả hiện ngay trên trang Checkout ("Thành tiền" tăng ~10 lần). AI mô tả *ý
định* của mã tốt hơn *hệ quả số học*.

**Thứ ba**, AI bỏ sót các lỗi phụ thuộc *luồng điều khiển* và *trạng thái*: khách vãng lai né giới
hạn số lần dùng coupon (nhánh thiếu `user_id`), hay lỗi giỏ mobile chỉ xuất hiện khi
`cart.length > 1`. Chi tiết đáng nhớ: lỗi "Thành tiền âm" (BUG-B-04) *không* tái hiện được với
coupon seed; phải tự tạo mã fixed lớn hơn đơn *qua tab Mã Giảm Giá của app admin* rồi áp trên
Checkout — một chuỗi thao tác UI mà AI ban đầu không lường.

**Nguyên tắc rút ra:** functional testing phải đi từ *đúng giao diện* mà tính năng phơi bày;
nhiều lỗi "nặng" (phân quyền admin, mật khẩu plaintext) chỉ chạm được từ API nên đã được loại
khỏi phạm vi. AI khuếch đại năng suất nhưng *không thay thế* việc con người chốt phạm vi, đọc mã
và *thao tác thật trên UI* để biến "dự đoán" thành "bằng chứng".

---

## Phần 2 — AI Audit Report

**Khai báo:** *"I use AI tools for the following tasks."*

### Interaction 1 — Domain Testing & BVA cho FR-01 (Đăng ký)
- **Công cụ:** Claude Code (Opus 4.8)
- **Ngày giờ:** 2026-07-01 21:57
- **Prompt:** `/domain-bva-test Do requirement 6 (HW02-Domain-Testing.md, Homework-Policies.md, CLAUDE.md)` — kèm hướng dẫn từng bước trong skill `domain-bva-test`, điền trực tiếp `main.tex` (tiếng Việt), chế độ "sinh viên tự chạy, AI dự đoán".
- **AI output:** Đọc `Register.jsx` (form Đăng ký) + lược đồ `users`; phân hoạch miền name/email/password, chọn điểm ON/OFF/IN cho biên độ dài, sinh các ca Domain + BVA, gap analysis, và các bug ứng viên trên form. Khiếm khuyết trọng tâm: regex bắt buộc khoảng trắng + cấm ký tự đặc biệt; email `type="text"` không kiểm định dạng; email không UNIQUE (trùng lặp).
- **Human review:** Sinh viên chịu trách nhiệm. Sau Interaction 6, phạm vi được chốt lại theo UI: giữ 4 bug quan sát được trên form (BUG-A-01..04); loại 2 khiếm khuyết chỉ chạm được từ API (body rỗng vẫn đăng ký, mật khẩu plaintext qua `/users/me`). Còn lại: screenshot + GitHub Issue.

### Interaction 2 — Domain Testing & BVA cho FR-09 (Mã giảm giá)
- **Công cụ:** Claude Code (Opus 4.8)
- **Ngày giờ:** 2026-07-01 22:12
- **Prompt:** `đọc đề và làm tiếp tục HW02-Domain-Testing.md` — dùng skill `domain-bva-test`, điền Chương Feature B, cùng style Feature A.
- **AI output:** Đọc luồng áp mã trên trang Checkout + bảng `coupons` (seed SAVE10/BIGBUY/VIP100/EXPIRED); phân hoạch code/total_amount/user_id/type/expiry, sinh các ca Domain + BVA, gap analysis, các bug ứng viên. Trọng tâm: công thức percent sai (~10x); off-by-one biên `min_order_amount` (`>` thay vì `>=`); khách vãng lai né giới hạn số lần dùng; "Thành tiền" không kẹp ≥ 0 với mã fixed.
- **Human review:** Sinh viên chịu trách nhiệm. Sau Interaction 6, giữ 4 bug quan sát được trên trang Checkout (BUG-B-01..04); loại lỗi giả mạo `user_id` (endpoint không xác thực) vì chỉ chạm được từ API; lỗi mã rỗng/chữ thường bị UI chặn nên không tái hiện từ giao diện. BUG-B-04 (final âm) được tái hiện bằng cách tạo mã fixed ở tab Mã Giảm Giá của app admin rồi áp trên Checkout. Còn lại: screenshot + GitHub Issue.

### Interaction 3 — Domain Testing & BVA cho FR-14 (Quản lý danh mục)
- **Công cụ:** Claude Code (Opus 4.8)
- **Ngày giờ:** 2026-07-01 22:20
- **Prompt:** `tiếp tục testing, phần feature B có dùng /domain-bva-test không, nếu không thì dùng rồi kiểm tra lại, làm phần c` — dùng skill, điền Chương Feature C.
- **AI output:** Ban đầu thiết kế theo endpoint `/api/categories`. Sau Interaction 6 được viết lại hoàn toàn theo app admin `frontend-admin`, tab "Danh mục" (chỉ có Thêm/Xóa): biến biên đổi từ `id` sang `name.length` của ô "Tên danh mục mới"; phân hoạch name (rỗng/khoảng trắng/trùng/độ dài) và trạng thái danh mục khi Xóa (có/không có sản phẩm). Trọng tâm: ô tên chấp nhận rỗng/khoảng trắng; cho phép trùng tên; xóa danh mục còn sản phẩm làm mồ côi; ô tên không giới hạn độ dài.
- **Human review:** Sinh viên chịu trách nhiệm. Giữ 4 bug quan sát được trên tab Danh mục (BUG-C-01..04); loại lỗi phân quyền admin ở tầng API (user thường CRUD được khi gọi thẳng API) và PUT/DELETE id không tồn tại trả 200 — cả hai chỉ chạm được từ API, không phải functional testing từ UI. Còn lại: screenshot + GitHub Issue.

### Interaction 4 — Domain Testing & BVA cho FR-07 (Giỏ hàng — Mobile)
- **Công cụ:** Claude Code (Opus 4.8)
- **Ngày giờ:** 2026-07-01 22:28
- **Prompt:** `tiếp tục /domain-bva-test` — dùng skill cho Feature D (mobile), điền Chương Feature D.
- **AI output:** Đọc `frontend-mobile/App.js` (`normalizeQuantity`, `addToCart`, ô sửa số lượng, `cartTotal`, `handleConfirmCheckout`); phân hoạch quantity/ô sửa giỏ/cart.length, sinh các ca Domain + BVA, gap analysis, 4 bug. Trọng tâm: ô sửa giỏ lưu `parsed+1` (off-by-one); checkout `slice(0,-1)` bỏ món cuối nhưng tính đủ tiền; ép âm thầm về 1; không giới hạn số lượng/tồn kho.
- **Human review:** Sinh viên chịu trách nhiệm. App mobile chính là UI nên toàn bộ 4 bug (BUG-D-01..04) đều nằm trong phạm vi; sau Interaction 6 chỉ đổi nhãn "xác nhận qua API" thành "quan sát trên app". Còn lại: screenshot trên app Expo + GitHub Issue.

### Interaction 5 — Thực thi & xác nhận toàn bộ ca kiểm thử (FR-01/09/14/07)
- **Công cụ:** Claude Code (Opus 4.8)
- **Ngày giờ:** 2026-07-02
- **Prompt:** `tiếp tục hoàn thành các feature bên dưới, đọc qua tất cả md trong codebase và sử dụng skill khi cần`.
- **AI output:** Thực thi thật toàn bộ ca đã thiết kế để chuyển cột Kết quả/Trạng thái từ "dự đoán" sang kết quả chạy thực, xác nhận từng khiếm khuyết ứng viên trên bốn feature. Điền Test Summary, mô tả Agent Skill, và AI Critique.
- **Human review:** Sinh viên đối chiếu output với cột Kết quả/Trạng thái; xác nhận không lỗi nào bị bịa. Lưu ý phiên này còn chạy ở tầng API — phạm vi được chuẩn hóa lại thành functional testing từ UI ở Interaction 6.

### Interaction 6 — Chuyển toàn bộ kiểm thử sang UI Frontend (theo chỉ đạo giảng viên)
- **Công cụ:** Claude Code (Opus 4.8)
- **Ngày giờ:** 2026-07-04
- **Prompt:** `không, không test api gì, tất cả đi từ UI theo đúng lời thầy` — kèm yêu cầu rà mọi feature và tài liệu nộp cho nhất quán ("sửa hết cho consistent").
- **Mục đích:** Sau khi giảng viên Hồ Tuấn Thanh trả lời trên diễn đàn Q&A HW02 rằng *functional testing phải thực hiện từ UI Frontend*, rà soát lại toàn bộ và chuyển mọi ca kiểm thử/bug từ tầng API sang thao tác trên giao diện.
- **AI output:** AI đọc lại mã 4 ứng dụng (`frontend-web`, `frontend-admin`, `frontend-mobile`, `backend`), phân loại từng ca/bug là *quan sát-được-từ-UI* hay *chỉ-API*. Phát hiện quan trọng: Feature C có app admin riêng `frontend-admin` (tab Danh mục, chỉ Thêm/Xóa) — đính chính nhận định sai trước đó rằng "không có UI admin". Kết quả rà soát: Feature A giữ 9 ca DT + 5 BVA, 4 bug (bỏ ca body rỗng, mật khẩu plaintext — chỉ API); Feature B giữ 9 DT + 6 BVA, 4 bug (bỏ giả mạo `user_id`, mã rỗng/chữ thường bị UI chặn; BUG-B-04 final âm nay tái hiện bằng cách tạo mã ở admin rồi áp trên Checkout); Feature C viết lại theo tab Danh mục (biến biên đổi từ `id` sang `name.length`); Feature D giữ nguyên (app mobile chính là UI), chỉ sửa nhãn "xác nhận qua API" thành "quan sát trên app". Tổng: **61 ca / 16 bug** (từ 20 bug trước đó). Cập nhật Test Summary, README, kịch bản quay video (demo trên trình duyệt thay vì công cụ dòng lệnh), và bản Markdown bug-report/critique.
- **Human review:** Sinh viên (Lý Quốc Thạnh) chịu trách nhiệm: quyết định giữ đúng tinh thần functional testing — loại hẳn các bug chỉ chạm được từ API (kể cả bug Critical như phân quyền admin, mật khẩu plaintext) thay vì ngụy trang thành ca UI. Các bug "vùng xám" (guest né giới hạn coupon, checkout mobile bỏ món cuối) được giữ lại kèm ghi chú trung thực về mức độ quan sát được trên màn hình.
