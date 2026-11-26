# Product Specification - OmniStudy

## 1. Tầm Nhìn Sản Phẩm (Product Vision)

**OmniStudy** được định vị là một **Hệ thống Quản trị Kế hoạch Ôn thi & Công việc Thông minh (Smart Exam & Task Planner)** dành cho sinh viên đại học và người tự học.

Ứng dụng giải quyết bài toán cốt lõi: **"Làm thế nào để không bị trễ hạn deadline và không phải ôn thi nhồi nhét (cramming) vào đêm trước ngày thi?"**

Bằng cách đưa ra một giải pháp tối giản, giao diện **Light-First Cozy Woodland Bento Grid** dịu mắt (lấy cảm hứng từ thiên nhiên rừng thông và mặt hồ tĩnh lặng với họa tiết lá cây tối giản, không dùng nhân vật game), OmniStudy kết hợp động cơ tự động tính điểm khẩn cấp theo deadline thời gian thực, đồng bộ thời gian biểu từ Google Calendar và bộ công cụ tự động phân rã đề cương môn thi thành các bài ôn theo ngày.

---

## 2. Chân Dung Người Dùng (User Personas)

### Persona: Sinh viên Đại học / Người tự học Lập trình (Primary Persona)
- **Độ tuổi:** 18 - 24 tuổi.
- **Đặc điểm hành vi:**
  - Có lịch học dày đặc trên trường kết hợp bài tập lớn (Assignments) và các kỳ thi giữa kỳ/cuối kỳ nối tiếp nhau.
  - Sử dụng Google Calendar để theo dõi lịch học và các cuộc hẹn cá nhân.
  - Thường xuyên gặp tình trạng "nước đến chân mới nhảy" do không nhìn thấy mức độ khẩn cấp thực tế của các deadline cách đó vài ngày.
- **Điểm đau (Pain Points):**
  1. **Độ ưu tiên tĩnh gây ảo giác an toàn:** Khi gán nhãn P1/P2 trong các ứng dụng Todo thông thường, một công việc P2 sắp hết hạn trong 3 giờ nữa vẫn nằm dưới công việc P1 còn tận 1 tuần nữa mới nộp.
  2. **Ôn thi bị động và dồn ứ:** Đề cương môn thi có 5-10 chủ đề lớn nhưng không biết cách chia nhỏ vào lịch hàng ngày để ôn dần, dẫn đến thức trắng đêm trước kỳ thi.
  3. **Phân mảnh giữa Lịch học và Danh sách việc:** Lịch học nằm trên Google Calendar, còn bài tập nằm ở ứng dụng khác hoặc sổ tay, không nhìn thấy bức tranh thời gian biểu tổng thể.

---

## 3. Giá Trị Cốt Lõi (Unique Selling Propositions - USP)

| Vấn đề của công cụ thông thường | Giải pháp của OmniStudy (USP) |
| :--- | :--- |
| **Độ ưu tiên tĩnh (Static Priority):** Task gắn nhãn P2 sẽ mãi đứng yên dưới P1 dù sắp đến giờ nộp. | **Dynamic Urgency Scoring Engine:** Điểm số khẩn cấp $U$ tăng theo hàm số phi tuyến tính khi số giờ còn lại giảm dần, tự động đẩy task lên đầu bảng và cập nhật chấm tròn trạng thái, viền mảnh `1px` cùng nhãn pill cảnh báo. |
| **Phân mảnh lịch học & công việc:** Phải mở nhiều tab để xem lịch Google và danh sách việc cần làm. | **Two-way Timeline Integration:** Tích hợp trực tiếp Google Calendar API v3 để đọc lịch học cá nhân và hiển thị song song cùng các task ôn thi trên Lịch tuần. |
| **Ôn thi bị động (Cramming):** Không biết phân bổ đề cương theo số ngày còn lại trước ngày thi. | **Smart Exam Task Generator:** Nhập môn thi và danh sách chủ đề, hệ thống tự động tính khoảng cách ngày và sinh ra các task ôn tập trải đều vào lịch trước ngày thi. |

---

## 4. Phạm Vi Sản Phẩm (Scope)

### Nằm trong phạm vi (In-Scope):
1. **Focus Dashboard (Cozy Woodland Bento Grid):** Thanh điều hướng trái `ForestSidebar` (`248px`), banner cảnh quan `ScenicHeaderBanner`, lưới Bento 12 cột gồm `ExamCountdownWidget` (5 cột, đếm ngược 4 cột trên 1 hàng ngang), `Daily Stats` (7 cột, tổng hợp giờ công việc trong ngày), `UrgencyTaskList` (8 cột) và `Mini Schedule` (4 cột).
2. **Weekly Calendar:** Xem thời gian biểu tuần (`Thứ 2 - CN`, hỗ trợ Time-blocking) tích hợp Google Calendar API v3 kèm dữ liệu mẫu dự phòng.
3. **Exam Prep Drawer:** Ngăn kéo trượt quản lý môn thi, checklist đề cương tính tỷ lệ sẵn sàng `% Readiness` và nút tự động sinh task ôn tập lên cả Dashboard và Lịch tuần.
4. **Design System, Motion & A11y:** Ngôn ngữ Light-First Cozy Woodland Bento, chuyển động mượt (`PageTransitionWrapper`, `ExamPrepDrawer`, `@media (prefers-reduced-motion: reduce)`), hỗ trợ bàn phím đầy đủ, Warm Shimmer Skeleton, Empty State và Error Alert.

### Không nằm trong phạm vi (Out-of-Scope):
- Không xây dựng backend riêng phức tạp (dùng Client-side Storage và Google Calendar API trực tiếp).
- Không tích hợp mạng xã hội, chat hay chia sẻ công việc nhóm (giữ ứng dụng tập trung cá nhân).
