# Báo Cáo Phân Tích & Bóc Tách 5 Lỗi "AI Slop" Khi Thiết Kế Giao Diện

- **Dự án:** OmniStudy - Smart Exam & Task Planner
- **Màn hình thử nghiệm:** Focus Dashboard (Bento Grid)
- **Công cụ sinh giao diện đối chứng:** Google Stitch / v0.dev
- **Mục tiêu:** Nhận diện và phân tích chi tiết 5 lỗi "AI Slop" điển hình khi các công cụ AI tự động sinh giao diện Dark/Neon/Glassmorphism rập khuôn, từ đó trình bày giải pháp thiết kế thủ công chuẩn **Light-First Cozy Woodland Bento**.

---

## 1. Prompt Thử Nghiệm Gửi Cho AI (Điển Hình Gây AI Slop)

```text
"Design a modern dark-mode productivity dashboard for a student exam and task planner called 'OmniStudy'. Include an exam countdown timer, a prioritized task list with urgency badges, and a daily schedule synced with Google Calendar. Use a sleek futuristic aesthetic."
```

![Minh họa 5 lỗi AI Slop và giải pháp Cozy Woodland Bento](./assets/buoi-1/ai-slop-review.svg)

---

## 2. Phân Tích 5 Lỗi "AI Slop" & Giải Pháp Chuẩn Cozy Woodland Bento

### Lỗi 1: Rung Lắc Bố Cục Khi Nhảy Số (Layout Shift do thiếu `tabular-nums`)
- **Biểu hiện của AI Slop:** AI sinh khối đồng hồ đếm ngược (`01d : 11h : 18m : 19s`) bằng font chữ tỷ lệ thông thường (`Inter` hoặc `Roboto`) và thường để các đơn vị tự xuống dòng (wrap chữ `giây`) khi thu hẹp khung Bento.
- **Lý do đây là lỗi nghiêm trọng:** Trong font tỷ lệ, độ rộng số `1` hẹp hơn đáng kể so với số `8` hay `0`. Mỗi giây đồng hồ cập nhật, toàn bộ thẻ đếm ngược bị giật ngang liên tục (Cumulative Layout Shift - CLS), gây mỏi mắt và thiếu chuyên nghiệp.
- **Giải pháp Cozy Woodland Bento:**
  - Áp dụng 100% font `'JetBrains Mono'` kèm `font-variant-numeric: tabular-nums;` cho mọi con số: đồng hồ đếm ngược, điểm khẩn cấp $U$, tỷ lệ sẵn sàng (`60%`), tổng giờ (`8.5h`) và khung giờ lịch tuần.
  - Khóa đồng hồ đếm ngược thành **1 hàng ngang duy nhất gồm 4 ô cố định** (`03 Ngày | 14 Giờ | 22 Phút | 45 Giây`) để triệt tiêu hoàn toàn hiện tượng xuống dòng chữ `Giây`.

---

### Lỗi 2: Lạm Dụng Nền Tối `#090A0F`, Glassmorphism Blur & Gradient Tím - Hồng Lòe Loẹt
- **Biểu hiện của AI Slop:** Khi gặp prompt chung chung về "modern productivity app", AI mặc định đổ nền tối đen `#090A0F`, phủ gradient tím - hồng (`#2E1065` -> `#831843`), thêm kính mờ `backdrop-filter: blur(20px)` và bóng đổ nặng `box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.75)`.
- **Lý do đây là lỗi nghiêm trọng:**
  - Phong cách dark-neon tím-hồng mang đậm dấu ấn giao diện mẫu AI rập khuôn, tạo cảm giác lạnh lẽo và gây căng thẳng thị giác khi sinh viên ôn thi nhiều giờ liên tục.
  - Hiệu ứng `blur()` làm mờ ranh giới cấu trúc Bento Grid, giảm độ tương phản chữ và tiêu tốn tài nguyên GPU khi cuộn trang.
- **Giải pháp Cozy Woodland Bento:**
  - Chuyển sang hệ màu **Light-First Cozy Woodland** lấy cảm hứng từ rừng thông yên tĩnh: nền tổng thể Warm Parchment `#ECE7D8`, thanh điều hướng trái Forest Sidebar `#2F4231` kết hợp biển tiêu đề gỗ `#9B6B43`, thẻ Bento Warm Cream `#FAF6EC` và thẻ con Soft Oat `#F1ECDD`.
  - Loại bỏ toàn bộ gradient tím-hồng và kính mờ, thay bằng viền cấu trúc phẳng sắc nét `1px solid #DDD6C3`.

---

### Lỗi 3: "Happy Path Fallacy" - Hoàn Toàn Bỏ Quên 3 Trạng Thái Biên & Kiểm Soát Form
- **Biểu hiện của AI Slop:** Mẫu AI chỉ vẽ trạng thái lý tưởng có sẵn dữ liệu đẹp. AI bỏ qua hoàn toàn trạng thái **Đang tải (Loading)**, **Rỗng (Empty)**, **Lỗi mạng / 401 (Error)**, đồng thời sinh ô nhập ngày giờ AM/PM thiếu ràng buộc hợp lệ.
- **Lý do đây là lỗi nghiêm trọng:** Khi mới mở ứng dụng (chưa có task), mạng chậm hoặc Access Token Google hết hạn (`401`), giao diện sẽ trắng xóa hoặc vỡ bố cục. Nếu form cho phép bấm gửi khi giờ ước tính $\le 0$ hoặc hạn chót ở quá khứ, dữ liệu sai sẽ làm hỏng công thức tính điểm $U$.
- **Giải pháp Cozy Woodland Bento:**
  - **Loading:** Hiện thực **Warm-Cream Shimmer Skeleton** (`@keyframes warmShimmer` quét mượt trên `#F1ECDD` / `#FAF6EC`) đúng kích thước thẻ thật.
  - **Empty:** Họa tiết lá cây tối giản nét `1.75px`, hướng dẫn rõ ràng và nút Primary CTA `+ Tạo Công Việc Đầu Tiên` (`#4F7043`).
  - **Error:** Khung cảnh báo nền `#FBEAE7`, viền `1px solid #B94435`, chữ `#9A3326` kèm nút `Thử lại` / `Đăng nhập lại Google`.
  - **Form Modals:** Chuẩn hóa nhập liệu **24 giờ (`HH:mm DD/MM/YYYY`)**, tự động khóa (`disabled`) nút Submit khi dữ liệu không hợp lệ và hiển thị lỗi màu `#9A3326`.

---

### Lỗi 4: Sụp Đổ Phân Cấp Thị Giác (Visual Hierarchy Collapse)
- **Biểu hiện của AI Slop:** AI tô sáng tất cả thẻ Bento, nút `Sửa`, nút `Xóa` và mọi nhãn ưu tiên bằng các màu neon phát sáng hoặc viền trái dày ngang nhau.
- **Lý do đây là lỗi nghiêm trọng:** Vi phạm nguyên tắc nhận thức thị giác. Người dùng không thể phân biệt ngay công việc sắp hết hạn trong 2.5 giờ tới với một công việc còn 30 giờ nữa mới cần làm.
- **Giải pháp Cozy Woodland Bento:**
  - Phân cấp tập trung bằng hệ **5 cấp độ Urgency Tiers** dùng viền mảnh `1px` + chấm trạng thái + organic pill badge (không dùng viền trái neon dày): chỉ `OVERDUE` (`#9A3326` / `#F9DFDA` / `#D97C6E`) và `EXTREME` (`#9E4718` / `#FBE6D4` / `#E09866`) mang sắc ấm cảnh báo cao; các mức `HIGH`, `MODERATE`, `LOW` giảm dần cường độ.
  - Nút hành động chính duy nhất dùng Pine Moss `#4F7043` (hover `#3F5B35`), nút phụ dùng nền `#F1ECDD` viền `#DDD6C3`.

---

### Lỗi 5: Độ Tương Phản Chữ Kém & Thiếu Tính Nhất Quán Số Liệu Thực Tế
- **Biểu hiện của AI Slop:** Trên nền tối `#11131A`, AI thường dùng chữ xám `#4B5563` cho nhãn phụ (tỷ lệ tương phản chỉ ~`2.8:1`, trượt chuẩn WCAG AA `4.5:1`), đồng thời điền các con số giả mạo không khớp nhau (ví dụ: tổng giờ thống kê hiển thị `3.5h` trong khi tổng các task bên dưới cộng lại ra `8.5h`, hoặc ngày trong tuần không khớp lịch thật).
- **Lý do đây là lỗi nghiêm trọng:** Người dùng khó đọc thông tin phụ dưới ánh sáng ban ngày, và số liệu mâu thuẫn làm mất hoàn toàn độ tin cậy của công cụ quản lý thời gian.
- **Giải pháp Cozy Woodland Bento:**
  - Trên nền `#FAF6EC`, chữ chính Forest Ink `#283629` đạt độ tương phản vượt chuẩn WCAG AAA (`> 11:1`), chữ phụ Sage Bark `#546353` đạt chuẩn WCAG AA (`> 5.8:1`), chữ trên Sidebar `#F4EFE2` / `#C6D1BF` tương phản rõ trên `#2F4231`.
  - Kiểm soát chặt chẽ dữ liệu nhất quán: Lịch tháng 11/2026 chuẩn xác (`Thứ Tư, 25/11/2026`, tuần `23/11 - 29/11`, thi `08:30 - Thứ Bảy, 28/11/2026`), tổng giờ `8.5h = 1.5h + 2.0h + 3.0h + 2.0h` và Readiness `60% = 3/5 chủ đề`.

---

## 3. Checklist Kiểm Duyệt Giao Diện Trước Khi Xuất Mã Nguồn

Mỗi màn hình và thành phần UI do AI hoặc lập trình viên tạo ra trong OmniStudy phải vượt qua 5 tiêu chí bắt buộc:
1. Toàn bộ số liệu, bộ đếm và giờ giấc đã dùng `'JetBrains Mono'` + `tabular-nums` và không bị xuống dòng chữ `Giây` chưa?
2. Bảng màu có tuân thủ đúng bộ tokens **Cozy Woodland Bento** (`#ECE7D8`, `#2F4231`, `#FAF6EC`, `#F1ECDD`, `#DDD6C3`, `#4F7043`) thay vì nền `#090A0F` hay gradient neon không?
3. Đã hiện thực đủ 3 trạng thái biên: `@keyframes warmShimmer` (Loading), Empty State có nút CTA, và Error Alert `#B94435` có nút Retry chưa?
4. Các Dialog Modal tạo Task / Lịch thi đã dùng chuẩn 24h (`HH:mm DD/MM/YYYY`) và khóa nút Submit (`disabled`) khi dữ liệu sai chưa?
5. Tỷ lệ tương phản chữ trên mọi bề mặt có đạt tối thiểu `4.5:1` (WCAG AA) và giữ tính nhất quán dữ liệu tuyệt đối chưa?
