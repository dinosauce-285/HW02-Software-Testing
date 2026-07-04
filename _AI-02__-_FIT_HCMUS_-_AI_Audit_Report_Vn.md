**Khoa Công nghệ Thông tin (FIT) – Trường Đại học Khoa học Tự nhiên (HCMUS)**

**CS423 / CSC13003 – Kiểm chứng Phần mềm (AI-augmented · 2026\)**

**CHÍNH SÁCH AI · BIỂU MẪU — 2026 v1.0**

# **AI Audit Report — Mẫu 5 mục cho mỗi Artifact**

*Phụ lục bắt buộc đính kèm cho mọi bài tập có dùng AI (HW\#01–HW\#06, Seminar).*

*Tài liệu được biên soạn lại từ Med Kharbach, PhD (2026) — Mẫu Chính sách Sử dụng AI cho Giáo dục Đại học. Giấy phép CC BY-NC-SA 4.0. Phiên bản này được FIT@HCMUS điều chỉnh cho môn CS423 / CSC15003 Kiểm chứng Phần mềm.*

## **1\. Thông tin Sinh viên**

| Mục | Giá trị |
| :---- | :---- |
| **Họ tên sinh viên (in hoa):** | LÝ QUỐC THẠNH |
| **MSSV:** | 23127262 |
| **Lớp / Khoá:** | 23KTPM2/23 |
| **Mã bài tập (ví dụ HW\#00, HW\#02):** | HW02 – Domain Testing on EShop |
| **Ngày làm bài:** | 01–02/07/2026 |
| **Công cụ AI đã dùng:** | Claude Opus 4.8 |
| **Công cụ AI đã dùng:** | \[x\] Có  \[ \] Không |

## **2\. Hướng dẫn (đọc trước khi điền)**

* Thêm 1 hàng cho mỗi artifact AI sinh (test case, script, checklist, OpenAPI spec, JMeter plan…).  
* Dán nguyên văn prompt — KHÔNG paraphrase.  
* Dán nguyên văn output AI (hoặc kèm screenshot có chú thích trong báo cáo).  
* Gắn nhãn: VALID / INVALID / INCOMPLETE.  
* Lý do phải dẫn chiếu slide, mục ISTQB, hoặc RFC kỹ thuật.  
* Hiển thị bản sửa với phần thay đổi được tô sáng.  
* Hàng mẫu in nghiêng — thay trước khi nộp.

## **3\. Bảng Audit — 1 hàng / artifact**

| (1) Prompt \+ Công cụ | (2) Output AI | (3) Verdict | (4) Lý do (ISTQB) | (5) Bản SV sửa |
| :---- | :---- | :---- | :---- | :---- |
| **Mẫu (italic) — thay trước khi nộp:** |  |  |  |  |
| **Tool: AI Tool (e.g., ChatGPT, Claude, Gemini)Thời gian: 14:32 25/02/2026Prompt:"Sinh test case cho hàm parsePhoneNumberVN…"** | TC01: parsePhoneNumberVN("0912345678")Kỳ vọng: {prefix:84, number:912345678, valid:true}… | INCOMPLETE | AI bỏ qua định dạng RFC 3966\. ISTQB FL §4.3 Boundary Value Analysis yêu cầu test ranh giới định dạng. | Thêm TC: parsePhoneNumberVN("+84-91-234-5678")Kỳ vọng: {prefix:84, number:912345678, valid:true} |
| **Artifact \#1Claude Opus 4.8 — skill domain-bva-test, Feature A (FR-01), Bước 3: bảng điểm ON/OFF/IN/OUT cho password.length (≥ 8).** | Bảng ghi IN-point \= "Aa1 bcdefgh", cột Độ dài \= 10\. | INVALID | Chuỗi "Aa1 bcdefgh" có 11 ký tự (đếm thủ công: A-a-1-space-b-c-d-e-f-g-h), không phải 10\. ISTQB FL (BVA/kiểm thử biên) yêu cầu điểm IN-point phải khớp đúng độ dài thực của giá trị mẫu, nếu không bảng ON/OFF/IN sẽ dẫn sai kỳ vọng khi thực thi ca kiểm thử. | Sửa Độ dài \= 11, giữ nguyên giá trị mẫu và kỳ vọng "Chấp nhận (trong miền)". |
| **Artifact \#2Claude Opus 4.8 — skill domain-bva-test, Feature A (FR-01), Bước 4: bảng Test cases Domain Testing.** | TC-A-DT-06 (email sai định dạng, BUG-A-03): Expected \= "Từ chối email không hợp lệ", Kết quả \= "Đăng ký OK", nhưng Trạng thái ghi Pass. | INVALID | Theo ISTQB FL, Trạng thái Pass/Fail phải phản ánh đúng việc Kết quả thực tế có khớp Expected Output hay không. Ở đây Kết quả khác Expected nên phải là Fail — AI tự mâu thuẫn ngay trong cùng một hàng dữ liệu. | Sửa Trạng thái thành Fail, khớp với các ca BUG-A-03 khác đã ghi nhận là lỗi thực sự. |
| **Artifact \#3Claude Opus 4.8 — skill domain-bva-test, Feature A (FR-01), Bước 5: câu kết luận độ phủ (EC → Test Case).** | Câu kết luận ghi: "4/4 lớp name, 5/5 lớp email, 9/9 lớp password đều có ít nhất một ca kiểm thử." | INVALID | Đối chiếu bảng phân hoạch lớp tương đương (Bước 2\) của biến email trong cùng tài liệu, số lớp EC được liệt kê là 6, không phải 5\. Kết luận độ phủ phải khớp số lớp đã định nghĩa trước đó, nếu không ma trận truy vết mất tính nhất quán. | Sửa lại thành "6/6 lớp email" cho khớp bảng phân hoạch lớp gốc. |
| **Artifact \#4Claude Opus 4.8 — skill domain-bva-test, Feature A (FR-01), bảng BVA Min/Min+1/Max-1/Max cho password.length.** | Bảng BVA ghi password.length: Min \= 7, Min+1 \= 9, ghi chú đi kèm: "Biên dưới đúng; off-point \= 7". | INVALID | Đặc tả yêu cầu password.length ≥ 8, nên biên dưới hợp lệ (điểm ON, Min) phải là 8; giá trị 7 chính là OFF-point (điểm ngoài miền) — mâu thuẫn với chính ghi chú trong cùng hàng của AI. | Sửa Min \= 8 (giữ nguyên Min+1 \= 9 và phần ghi chú). |
| **Artifact \#5Claude Opus 4.8 — skill domain-bva-test, Feature A (FR-01), mẫu báo cáo lỗi BUG-A-06.** | Trường "Mức độ" của BUG-A-06 (mật khẩu lưu dạng văn bản thuần, lộ qua GET /api/users/me và danh sách admin) được AI ghi là Minor. | INVALID | Theo ISTQB FL (phân loại mức độ nghiêm trọng lỗi) và nguyên tắc bảo mật cơ bản, lỗi lộ mật khẩu dạng plaintext ảnh hưởng toàn bộ người dùng và có thể dẫn đến chiếm đoạt tài khoản hàng loạt; mức độ hợp lý là Critical, không phải Minor. | Sửa Mức độ \= Critical. |

## **4\. Tổng kết Độ chính xác AI**

Tổng hợp verdict từ Mục 3 và điền vào bảng dưới.

| Chỉ số | Số lượng | Tỉ lệ |
| :---- | :---- | :---- |
| **Tổng artifact AI sinh đã audit** | 5 | \- |
| **VALID (đúng, dùng nguyên)** | 0 | %0% |
| **INVALID (sai; loại bỏ)** | 5 | %100% |
| **INCOMPLETE (chấp nhận sau khi sửa)** | 0 | %0% |

## **5\. Kết luận — Khi nào nên / không nên dùng AI?**

Qua đối chiếu bản do AI (Claude Opus 4.8) sinh trực tiếp vào main.tex với bản đã được sinh viên kiểm tra và thực thi, phát hiện 5/5 artifact chứa lỗi số liệu hoặc mâu thuẫn nội tại: đếm sai độ dài chuỗi mẫu, gán sai trạng thái Pass/Fail dù kết quả không khớp kỳ vọng, số lớp tương đương không khớp giữa hai mục trong cùng tài liệu, đảo lẫn Min và OFF-point trong bảng BVA, và hạ thấp mức độ nghiêm trọng của lỗi bảo mật lộ mật khẩu dạng văn bản thuần. AI mạnh ở việc dựng khung Domain Testing/BVA và diễn đạt mạch lạc, nhưng yếu ở việc tự kiểm tra số liệu định lượng và tính nhất quán giữa các phần. Khuyến nghị: luôn đối chiếu thủ công mọi con số, nhãn Pass/Fail và mức độ nghiêm trọng trước khi nộp.

## **6\. Mandatory Disclosure (dán nguyên văn)**

*"Bộ ca kiểm thử Domain Testing & BVA (Feature A – FR-01, HW02) này được sinh phiên bản đầu bởi Claude Opus 4.8; tôi đã rà soát và chỉnh sửa toàn bộ số liệu định lượng và nhãn Pass/Fail, bổ sung kiểm tra chéo độ dài chuỗi biên và mức độ nghiêm trọng của lỗi bảo mật; việc thực thi thật các ca kiểm thử trên backend EShop và xác nhận lỗi do tôi tự thực hiện. AI Audit Report chi tiết đính kèm ở Phụ lục A. Tôi cam đoan không dùng AI để sinh bất kỳ artifact nào thuộc danh mục bị cấm."*

## **Chữ ký**

| Họ tên sinh viên (in hoa): | LÝ QUỐC THẠNH |
| :---- | :---- |
| **MSSV:** | 23127262 |
| **Lớp / Khoá:** | 23KTPM2/23 |
| **Môn học:** | CS423 / CSC13003 – Kiểm chứng Phần mềm |
| **Giảng viên:** | Lâm Quang Vũ / Trương Phước Lộc / Hồ Tuấn Thanh |
| **Ngày:** | 04/07/2026 |
| **Chữ ký:** | LÝ QUỐC THẠNH |

## **Tham khảo**

* Kharbach, M. (2026). AI Use Policy Templates for Higher Education. CC BY-NC-SA 4.0.  
* ISTQB Foundation Level Syllabus (latest version).  
* Hardman, P. (2025). A Post-AI Learning Taxonomy.  
* Fuster Rabella, M. (2025). OECD Education Working Paper No. 338\.  
* Perkins, M., Roe, J., & Furze, L. (2025). AI Assessment Scale.  
* Anthropic (2025). Building reliable AI test agents — engineering blog.  
* DeepEval & Promptfoo documentation — testing frameworks for LLM systems.