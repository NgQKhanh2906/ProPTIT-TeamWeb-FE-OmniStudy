# Functional & Technical Specification - OmniStudy

Tài liệu này đặc tả toàn bộ yêu cầu chức năng (Functional Requirements), công thức nghiệp vụ, quy chuẩn định dạng dữ liệu và yêu cầu phi chức năng cho ứng dụng **OmniStudy (Smart Exam & Task Planner)** theo ngôn ngữ thiết kế **Light-First Cozy Woodland Bento**.

---

## 1. Yêu Cầu Chức Năng (Functional Requirements)

### 1.1. Phân Hệ Quản Lý Công Việc & Động Cơ Điểm Khẩn Cấp (Task & Urgency Engine)

- **FR-TASK-01 (Quản lý Công việc & Form Validation):**
  - Cho phép người dùng Tạo, Sửa, Xóa và Đánh dấu hoàn thành công việc thông qua `TaskFormDialog`.
  - Mỗi công việc bao gồm: Tiêu đề (`title`), Hạn chót (`deadline` theo định dạng **24 giờ `DD/MM/YYYY HH:mm`**), Thời lượng ước tính (`estimatedHours`), Độ ưu tiên gốc (`basePriority`: 1 đến 4, trong đó 4 là cao nhất), Nhãn môn học (`courseTag`), Trạng thái (`isCompleted`).
  - **Kiểm tra hợp lệ trực tiếp (Live Validation):** Sử dụng React Hook Form + Zod; nút gửi (`Lưu công việc`) **bắt buộc bị vô hiệu hóa (`disabled`)** khi bất kỳ trường nào chưa hợp lệ (tiêu đề rỗng, `estimatedHours <= 0`, hạn chót sai định dạng hoặc nằm trong quá khứ) và hiển thị thông báo lỗi bằng màu `--semantic-danger` (`#B94435`).
- **FR-TASK-02 (Công thức tính điểm khẩn cấp động - Dynamic Urgency Score):**
  - Hệ thống tự động tính toán lại điểm số khẩn cấp $U \in [0, 100]$ của mọi công việc chưa hoàn thành.
  - Công thức:
    $$U = \min\left(100, (BasePriority \times 20) + \left(\frac{EstimatedHours}{\max(h, 0.5)} \times 15\right)\right)$$
    *Trong đó: $h$ là số giờ còn lại từ thời điểm hiện tại tới hạn chót ($h = \frac{\text{deadline} - \text{now}}{3600000}$). Nếu $h \le 0$, công việc được phân loại là `OVERDUE`.*
- **FR-TASK-03 (Phân loại cấp bậc thị giác - Urgency Tiers):**
  - Mỗi thẻ công việc sử dụng **viền mảnh `1px solid` + chấm tròn trạng thái (`status dot`) + nhãn pill hữu cơ (`rounded-full`)**, tuyệt đối không dùng viền trái neon dày:
    - `OVERDUE` ($h \le 0$): Chữ & dot `#9A3326`, nền pill `#F9DFDA`, viền mảnh `1px solid #D97C6E` (kèm hiệu ứng nhịp thở nhẹ nhàng).
    - `EXTREME` ($U \ge 80$): Chữ & dot `#9E4718`, nền pill `#FBE6D4`, viền mảnh `1px solid #E09866`, tự động đưa lên đầu danh sách.
    - `HIGH` ($60 \le U < 80$): Chữ & dot `#8A580F`, nền pill `#FBF0D6`, viền mảnh `1px solid #D9AE59`.
    - `MODERATE` ($35 \le U < 60$): Chữ & dot `#2B566B`, nền pill `#E4F0F5`, viền mảnh `1px solid #7FAAC0`.
    - `LOW` ($U < 35$): Chữ & dot `#546353`, nền pill `#EAE6D9`, viền mảnh `1px solid #C9C2AE`.
- **FR-TASK-04 (Tự động sắp xếp & Tổng hợp Daily Stats):**
  - Danh sách công việc mặc định luôn được sắp xếp theo thứ tự giảm dần của điểm khẩn cấp $U$. Khi tick hoàn thành, công việc chuyển sang trạng thái gạch ngang chữ (`--text-muted`: `#626F5F`), trôi xuống cuối danh sách và không tính vào tổng giờ đang mở.
  - Thẻ **Daily Stats** (`7 cột`) tính toán và cập nhật tức thì tổng số giờ của các task đang hoạt động ($\sum EstimatedHours$), ví dụ bộ dữ liệu mẫu ngày `Thứ Tư, 25/11/2026` gồm 4 task có tổng chính xác `8.5h` (`1.5h + 2.0h + 3.0h + 2.0h`), hiển thị bằng `JetBrains Mono` `tabular-nums`.

---

### 1.2. Phân Hệ Đồng Bộ Lịch Cá Nhân Google Calendar (Google Calendar Integration)

- **FR-GCAL-01 (Xác thực Google OAuth2 Client):**
  - Cho phép người dùng đăng nhập tài khoản Google một chạm bằng `@react-oauth/google` để cấp quyền đọc lịch cá nhân (`https://www.googleapis.com/auth/calendar.events.readonly`).
  - Lưu trữ Access Token an toàn tại Client trong phiên làm việc.
- **FR-GCAL-02 (Đọc và Hiển thị sự kiện theo Tuần chuẩn):**
  - Gọi endpoint `GET https://www.googleapis.com/calendar/v3/calendars/primary/events` với tham số khoảng thời gian của tuần đang chọn (`timeMin`, `timeMax`).
  - Khung Lịch tuần (Weekly Calendar Grid) hiển thị chính xác 7 ngày từ **Thứ 2 đến Chủ Nhật** theo định dạng giờ `24h` (ví dụ tuần chuẩn: `Thứ 2 (23/11) - CN (29/11)`, làm nổi bật ngày hiện tại `Thứ Tư, 25/11/2026`).
  - Sự kiện Google Calendar được ánh xạ với hệ màu thông tin dịu `--semantic-info` (`#3A6F88`, nền `#E5F1F6`, chữ `#2B566B`).
- **FR-GCAL-03 (Lập kế hoạch thời gian - Time-blocking):**
  - Hiển thị trực quan các khoảng thời gian trống giữa các sự kiện Google Calendar trong ngày trên cả màn Lịch tuần và widget `Mini Schedule` (`4 cột`), cho phép ấn định khung giờ thực hiện cho từng Task.
- **FR-GCAL-04 (Cơ chế Dự phòng - Fallback Mode):**
  - Nếu người dùng chưa đăng nhập Google hoặc mất kết nối mạng, hệ thống cung cấp dữ liệu mẫu từ `mockGoogleCalendar.json` để toàn bộ giao diện lịch vẫn hiển thị và hoạt động bình thường.

---

### 1.3. Phân Hệ Quản Lý Lịch Thi & Bộ Sinh Lộ Trình (Exam Prep Kit)

- **FR-EXAM-01 (Quản lý Kỳ thi):**
  - Cho phép tạo và theo dõi các kỳ thi thông qua `ExamFormDialog` với định dạng thời gian `24h` (ví dụ: `08:30 - Thứ Bảy, 28/11/2026`, `Phòng 402 - Giảng đường A2`).
  - Các trường dữ liệu: Tên môn thi (`courseName`), Thời gian thi (`examDate`), Phòng thi (`room`), Trọng số điểm (`weight`), Danh sách các chủ đề cần ôn tập (`topics`). Nút Submit bị vô hiệu hóa khi thông tin kỳ thi không hợp lệ.
- **FR-EXAM-02 (Đồng hồ đếm ngược kỳ thi - Countdown Widget):**
  - Tính toán và hiển thị đồng hồ đếm ngược thời gian thực tới môn thi diễn ra gần nhất trên **1 hàng ngang 4 cột duy nhất** (`Ngày | Giờ | Phút | Giây`, không xuống dòng nhãn `Giây`).
  - Bắt buộc áp dụng `--font-mono` (`JetBrains Mono`) kết hợp thuộc tính `font-variant-numeric: tabular-nums` để ngăn ngừa rung giật giao diện (`CLS = 0`).
- **FR-EXAM-03 (Tỷ lệ sẵn sàng cho kỳ thi - Readiness Score):**
  - Tính toán phần trăm sẵn sàng theo thời gian thực khi người dùng tick chọn chủ đề trong `ExamPrepDrawer`:
    $$\text{Readiness \%} = \left(\frac{\text{Số chủ đề đã hoàn thành}}{\text{Tổng số chủ đề của môn thi}}\right) \times 100\%$$
  - Hiển thị thanh tiến trình màu `--semantic-success` (`#2E7D52` trên nền `#E3F2EA`). Ví dụ chuẩn: hoàn thành `3/5` chủ đề cho kết quả `60%`, còn lại `2` chủ đề chưa hoàn thành.
- **FR-EXAM-04 (Động cơ tự động sinh Task ôn tập - Smart Study Generator):**
  - Khi người dùng nhấn nút *"Tự động phân bổ task ôn vào lịch"* (hoặc *"Tự động tạo lộ trình ôn tập"*) trên một môn thi:
    1. Hệ thống tính số ngày còn lại trước ngày thi: $N = \text{daysUntilExam}$.
    2. Lấy danh sách $M$ chủ đề chưa hoàn thành.
    3. Tự động sinh ra $M$ công việc mới (`[Ôn thi <Tên Môn>] <Tên Chủ Đề>`), chia đều hạn chót vào các ngày trước kỳ thi với `basePriority = 3`, cập nhật đồng thời lên bảng `UrgencyTaskList` trên Dashboard và Lịch tuần kèm thông báo Toast.

---

## 2. Yêu Cầu Phi Chức Năng (Non-Functional Requirements)

1. **Hiệu năng & Độ ổn định Bố cục (Performance & CLS = 0):**
   - Cô lập state đồng hồ đếm ngược trong `useCountdownTimer` / `ExamCountdownWidget` để không gây re-render toàn bộ bảng Task mỗi giây.
   - Áp dụng `tabular-nums` trên toàn bộ con số và khung **Warm Shimmer Skeleton** (`@keyframes warmShimmer`) đúng kích thước thẻ thật để giữ Cumulative Layout Shift bằng `0`.
   - Điểm Lighthouse mục **Performance** trên bản build Production đạt $\ge 85$.
2. **Khả năng tiếp cận (Accessibility - a11y):**
   - Tỷ lệ tương phản chữ trên nền sáng `#FAF6EC` / `#ECE7D8` (`#283629`, `#546353`) và trên `ForestSidebar` `#2F4231` (`#F4EFE2`, `#C6D1BF`) đạt chuẩn WCAG AA/AAA.
   - Điều hướng hoàn toàn bằng bàn phím (`Tab`, `Enter`, `Escape` để đóng modal/drawer) với vòng focus màu `--border-focus` (`#4F7043`).
   - Bắt buộc hỗ trợ `@media (prefers-reduced-motion: reduce)` để vô hiệu hóa chuyển động ambient và thu gọn transition cho người dùng nhạy cảm chuyển động.
   - Điểm Lighthouse mục **Accessibility** đạt $\ge 90$.
3. **Thiết kế Responsive & Shell Layout:**
   - Desktop ($\ge 1024px$): `ForestSidebar` (`248px`) cố định bên trái + `ScenicHeaderBanner` + lưới Bento 12 cột (`Row 1`: 5 cột + 7 cột; `Row 2`: 8 cột + 4 cột).
   - Tablet / Mobile ($< 1024px$ / $< 768px$): `ForestSidebar` chuyển thành ngăn kéo trượt (mobile drawer), lưới Bento 12 cột tự động xếp chồng 1 cột mượt mà.
4. **Kiểm tra kiểu dữ liệu nghiêm ngặt (Strict TypeScript):**
   - 100% mã nguồn không sử dụng kiểu `any`. Toàn bộ props, state và dữ liệu API trả về phải có type/interface rõ ràng trong `src/types/`.
