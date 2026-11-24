# OmniStudy - Tài Liệu Đặc Tả Dự Án

## 1. Giới Thiệu Chung

Tài liệu này cung cấp mục lục và tóm tắt toàn bộ hệ thống đặc tả của dự án **OmniStudy (Smart Exam & Task Planner)**, được tối ưu hóa cho bài tập lớn xuyên suốt khóa học Frontend 2026.

---

## 2. Danh Mục Tài Liệu Kỹ Thuật (Documentation Index)

1. [../README.md](../README.md): Tổng quan dự án, tính năng cốt lõi, tech stack và hướng dẫn khởi chạy nhanh.
2. [../DESIGN.md](../DESIGN.md): Hệ thống Design Tokens (Màu sắc, Typography, Spacing, Radius) và bộ quy tắc phòng chống AI Slop.
3. [wireframes.md](./wireframes.md): Sơ đồ User Flow và bản vẽ Wireframe 3 màn hình chính kèm 3 trạng thái bắt buộc (Loading, Empty, Error).
4. [ai-review.md](./ai-review.md): Báo cáo phân tích 5 lỗi AI Slop khi sinh giao diện và giải pháp kỹ thuật khắc phục.
5. [spec.md](./spec.md): Đặc tả chi tiết nghiệp vụ, công thức tính điểm Urgency Score và tích hợp Google Calendar API.
6. [roadmap.md](./roadmap.md): Chi tiết lộ trình 7 buổi học kèm tiêu chuẩn nghiệm thu và hướng dẫn thực hành từng bước.

---

## 3. Bản Đồ Phân Hệ Chức Năng (Core Feature Modules)

1. **Task & Urgency Engine:** Quản lý công việc và thuật toán tính điểm khẩn cấp theo hạn chót (Deadline) và thời lượng ước tính.
2. **Google Calendar Sync:** Kết nối và hiển thị sự kiện thời gian biểu cá nhân qua Google Calendar API v3 (OAuth2 Client).
3. **Smart Exam Prep Kit:** Quản lý lịch thi, đồng hồ đếm ngược môn thi sắp tới và cơ chế tự động phân bổ bài ôn thành các task theo ngày.
