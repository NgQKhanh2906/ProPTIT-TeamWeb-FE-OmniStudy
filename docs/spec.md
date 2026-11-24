# Functional & Technical Specification - OmniStudy

Tài liệu này đặc tả toàn bộ yêu cầu chức năng (Functional Requirements), công thức nghiệp vụ và yêu cầu phi chức năng cho ứng dụng **OmniStudy (Smart Exam & Task Planner)**.

---

## 1. Yêu Cầu Chức Năng (Functional Requirements)

### 1.1. Phân Hệ Quản Lý Công Việc & Động Cơ Điểm Khẩn Cấp (Task & Urgency Engine)

- **FR-TASK-01 (Quản lý Công việc):**
  - Cho phép người dùng Tạo, Sửa, Xóa và Đánh dấu hoàn thành công việc.
  - Mỗi công việc bao gồm: Tiêu đề (`title`), Hạn chót (`deadline`), Thời lượng ước tính (`estimatedHours`), Độ ưu tiên gốc (`basePriority`: 1 đến 4, trong đó 4 là cao nhất), Nhãn môn học (`courseTag`), Trạng thái (`isCompleted`).
- **FR-TASK-02 (Công thức tính điểm khẩn cấp động - Dynamic Urgency Score):**
  - Hệ thống tự động tính toán lại điểm số khẩn cấp $U \in [0, 100]$ của mọi công việc chưa hoàn thành.
  - Công thức:
    $$U = (BasePriority \times 20) + \left(\frac{EstimatedHours}{\max(h, 0.5)} \times 15\right)$$
    *Trong đó: $h$ là số giờ còn lại từ thời điểm hiện tại tới hạn chót ($h = \frac{\text{deadline} - \text{now}}{3600000}$). Nếu $h \le 0$, task được đánh dấu là `OVERDUE`.*
- **FR-TASK-03 (Phân loại cấp bậc thị giác - Urgency Tiers):**
  - `OVERDUE` ($h \le 0$): Viền đỏ `#EF4444`, nhãn đỏ rực, hiệu ứng nhịp thở cảnh báo trễ hạn.
  - `EXTREME` ($U \ge 80$): Viền cam `#F97316`, tự động đưa lên vị trí ưu tiên số 1 của bảng việc.
  - `HIGH` ($60 \le U < 80$): Viền vàng `#EAB308`.
  - `MODERATE` ($35 \le U < 60$): Viền xanh dương `#3B82F6`.
  - `LOW` ($U < 35$): Viền xám `#4B5563`.
- **FR-TASK-04 (Tự động sắp xếp):** Danh sách công việc mặc định luôn được sắp xếp theo thứ tự giảm dần của điểm khẩn cấp $U$. Khi người dùng tick hoàn thành, task chuyển sang trạng thái gạch ngang chữ và trôi xuống đáy danh sách.

---

### 1.2. Phân Hệ Đồng Bộ Lịch Cá Nhân Google Calendar (Google Calendar Integration)

- **FR-GCAL-01 (Xác thực Google OAuth2 Client):**
  - Cho phép người dùng đăng nhập tài khoản Google một chạm bằng `@react-oauth/google` để cấp quyền đọc lịch cá nhân (`https://www.googleapis.com/auth/calendar.events.readonly`).
  - Lưu trữ Access Token an toàn tại Client trong phiên làm việc.
- **FR-GCAL-02 (Đọc và Hiển thị sự kiện):**
  - Gọi endpoint `GET https://www.googleapis.com/calendar/v3/calendars/primary/events` với tham số khoảng thời gian của tuần đang chọn (`timeMin`, `timeMax`).
  - Ánh xạ các sự kiện từ Google Calendar lên khung Lịch tuần (Weekly Calendar Grid) với màu xanh lam (`#3B82F6`) và nhãn Google Calendar.
- **FR-GCAL-03 (Lập kế hoạch thời gian - Time-blocking):**
  - Hiển thị trực quan các khoảng thời gian trống giữa các sự kiện Google Calendar trong ngày, cho phép người dùng ấn định khung giờ thực hiện cho từng Task.
- **FR-GCAL-04 (Cơ chế Dự phòng - Fallback Mode):**
  - Nếu người dùng chưa đăng nhập Google hoặc mất kết nối mạng, hệ thống cung cấp dữ liệu mẫu từ `mockGoogleCalendar.json` để toàn bộ giao diện lịch vẫn hiển thị và hoạt động bình thường.

---

### 1.3. Phân Hệ Quản Lý Lịch Thi & Bộ Sinh Lộ Trình (Exam Prep Kit)

- **FR-EXAM-01 (Quản lý Kỳ thi):**
  - Cho phép tạo và theo dõi các kỳ thi: Tên môn thi (`courseName`), Thời gian thi (`examDate`), Phòng thi (`room`), Trọng số điểm (`weight`), Danh sách các chủ đề/bài học cần ôn tập (`topics: string[]`).
- **FR-EXAM-02 (Đồng hồ đếm ngược kỳ thi - Countdown Widget):**
  - Tính toán và hiển thị đồng hồ đếm ngược thời gian thực (Ngày : Giờ : Phút : Giây) tới môn thi diễn ra gần nhất.
  - Bắt buộc áp dụng font số `JetBrains Mono` kết hợp thuộc tính `font-variant-numeric: tabular-nums` để ngăn ngừa rung giật giao diện.
- **FR-EXAM-03 (Tỷ lệ sẵn sàng cho kỳ thi - Readiness Score):**
  - Tính toán phần trăm sẵn sàng:
    $$\text{Readiness \%} = \left(\frac{\text{Số chủ đề đã hoàn thành task ôn}}{\text{Tổng số chủ đề của môn thi}}\right) \times 100\%$$
  - Hiển thị thanh tiến trình màu Xanh lá (`#10B981`) trên thẻ môn thi.
- **FR-EXAM-04 (Động cơ tự động sinh Task ôn tập - Smart Study Generator):**
  - Khi người dùng nhấn nút *"Tự động tạo lộ trình ôn tập"* trên một môn thi:
    1. Hệ thống tính số ngày còn lại trước ngày thi: $N = \text{daysUntilExam}$.
    2. Lấy danh sách $M$ chủ đề chưa hoàn thành.
    3. Tự động sinh ra $M$ công việc (Tasks) mới, gán tên `[Ôn thi <Tên Môn>] <Tên Chủ Đề>`, chia đều hạn chót vào các ngày trước kỳ thi, và tự động đẩy vào bảng Task chung với `basePriority = 3`.

---

## 2. Yêu Cầu Phi Chức Năng (Non-Functional Requirements)

1. **Hiệu năng & Tối ưu Render (Performance):**
   - Không gây re-render toàn bộ bảng Task khi đồng hồ đếm ngược nhảy giây (tách riêng Component đếm ngược hoặc cô lập State).
   - Điểm Lighthouse mục **Performance** trên bản build Production đạt $\ge 85$.
2. **Khả năng tiếp cận (Accessibility - a11y):**
   - Tỷ lệ tương phản chữ đạt chuẩn WCAG AA (tối thiểu `4.5:1` cho văn bản thông thường, `3:1` cho chữ lớn và nhãn đồ họa).
   - Dùng được hoàn toàn bằng bàn phím (phím `Tab`, `Enter`, `Escape` để đóng mở modal).
   - Điểm Lighthouse mục **Accessibility** đạt $\ge 90$.
3. **Thiết kế Responsive (Mobile-First):**
   - Khung lưới Bento Grid 12 cột tự động chuyển thành cột đơn xếp chồng mượt mà trên màn hình điện thoại ($< 768px$).
4. **Kiểm tra kiểu dữ liệu nghiêm ngặt (Strict TypeScript):**
   - 100% mã nguồn không sử dụng kiểu `any`. Toàn bộ props và dữ liệu API trả về phải có type/interface rõ ràng.
