# figures/ — Screenshot bug

Đặt ảnh chụp mỗi bug vào thư mục này theo **đúng tên** dưới đây (main.tex đã trỏ sẵn qua
`\graphicspath{{./figures/}}`). Sau khi thêm ảnh, **bỏ dấu `%`** ở dòng `\includegraphics`
tương ứng trong `main.tex` để ảnh hiện trong PDF.

| Bug | Tên file | Chụp gì |
|-----|----------|---------|
| BUG-A-01 | `bug_a_01.png` | Trang đăng ký báo "Mật khẩu quá yếu!" với `Password123!` |
| BUG-A-02 | `bug_a_02.png` | Đăng ký thành công với `Passw0rd 1` |
| BUG-A-03 | `bug_a_03.png` | Đăng ký thành công với email `notanemail` |
| BUG-A-04 | `bug_a_04.png` | Response tạo user thứ 2 với `test@eshop.com` |
| BUG-A-05 | `bug_a_05.png` | `POST /api/register` body `{}` → 200 (Postman/curl) |
| BUG-A-06 | `bug_a_06.png` | `GET /api/users/me` trả `password` plaintext |
| BUG-B-01 | `bug_b_01.png` | `apply-coupon SAVE10` → final 4.000.000 |
| BUG-B-02 | `bug_b_02.png` | `VIP100` total 300.000 → 400 "chưa đủ" |
| BUG-B-03 | `bug_b_03.png` | `VIP100` không user_id vẫn áp dù đã đạt giới hạn |
| BUG-B-04 | `bug_b_04.png` | `apply-coupon` user_id giả, không token → success |
| BUG-B-05 | `bug_b_05.png` | Mã fixed discount>min → final âm (-40.000) |
| BUG-C-01 | `bug_c_01.png` | `POST /api/categories` bằng token user → 200 |
| BUG-C-02 | `bug_c_02.png` | Tạo category `{name:""}` hoặc `{}` → 200 |
| BUG-C-03 | `bug_c_03.png` | Tạo category "Laptop" trùng → 200 |
| BUG-C-04 | `bug_c_04.png` | `DELETE /api/categories/999` → 200 "deleted" |
| BUG-C-05 | `bug_c_05.png` | Sau khi xóa category 1, products còn category_id=1 |
| BUG-D-01 | `bug_d_01.png` | Giỏ: sửa số lượng 2 → hiển thị 3 |
| BUG-D-02 | `bug_d_02.png` | Đơn 3 món → lịch sử chỉ 2 món, tổng tính 3 |
| BUG-D-03 | `bug_d_03.png` | Nhập qty `0`/`abc` → giỏ có qty 1 |
| BUG-D-04 | `bug_d_04.png` | Nhập qty `999999` → giỏ chấp nhận |

**Mẹo chụp API (A, B, C):** dùng Postman, hoặc chụp terminal chạy `curl`. Backend:
`cd eshop-sut/backend && node database.js && node server.js` (cổng 3000).
**Feature D (mobile):** chạy `cd eshop-sut/frontend-mobile && npx expo start`, mở Expo Go /
emulator để chụp màn hình giỏ hàng.

> Ảnh không được commit ở bản này (thư mục để trống ngoài README) — sinh viên tự thêm.
