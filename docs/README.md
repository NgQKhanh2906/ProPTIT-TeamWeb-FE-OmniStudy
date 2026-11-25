# OmniStudy - Tài Liệu Đặc Tả Dự Án

## 1. Giới Thiệu Chung

Tài liệu này cung cấp mục lục và tóm tắt toàn bộ hệ thống đặc tả của dự án **OmniStudy (Smart Exam & Task Planner)**, được thiết kế cho bài tập lớn cá nhân xuyên suốt khóa học **Frontend Development 2026 — ProPTIT Web**.

---

## 2. Danh Mục Tài Liệu Kỹ Thuật (Documentation Index)

### 2.1. Tài Liệu Sản Phẩm, Thiết Kế & Lộ Trình Cốt Lõi
1. [../README.md](../README.md): Tổng quan dự án, tính năng cốt lõi, tech stack và hướng dẫn khởi chạy nhanh.
2. [../DESIGN.md](../DESIGN.md): Hệ thống Design Tokens (Màu sắc, Typography, Spacing, Radius) và bộ quy tắc phòng chống AI Slop.
3. [product.md](./product.md): Tầm nhìn sản phẩm, chân dung người dùng, USP và phạm vi In-Scope / Out-of-Scope.
4. [spec.md](./spec.md): Đặc tả chi tiết nghiệp vụ, công thức tính điểm Urgency Score, Google Calendar API v3 và Exam Prep Kit.
5. [wireframes.md](./wireframes.md): Sơ đồ User Flow và bản vẽ Wireframe 3 màn hình chính kèm 3 trạng thái bắt buộc (Loading, Empty, Error).
6. [ai-review.md](./ai-review.md): Báo cáo phân tích 5 lỗi AI Slop khi sinh giao diện và giải pháp kỹ thuật khắc phục.
7. [roadmap.md](./roadmap.md): Chi tiết lộ trình 7 buổi học kèm tiêu chuẩn nghiệm thu và đầu việc thực hành từng bước.

### 2.2. Tài Liệu Kiến Trúc, Vận Hành & Tiến Độ
8. [architecture.md](./architecture.md): Kiến trúc Client-side SPA (React 18 + Vite + TanStack Query v5) và luồng đồng bộ Google Calendar.
9. [folder-structure.md](./folder-structure.md): Cấu trúc cây thư mục mã nguồn Vite + React + TypeScript và vai trò từng module.
10. [deploy.md](./deploy.md): Hướng dẫn đóng gói và triển khai lên GitHub Pages (Buổi 2) và Vercel / Netlify (Buổi 7).
11. [git-workflow.md](./git-workflow.md): Quy trình phân nhánh Git và chuẩn viết Commit Message cho lộ trình 7 buổi.
12. [worklog.md](./worklog.md): Bảng theo dõi tiến độ thực thi từ Buổi 1 đến Buổi 7.
13. [tai_lieu_chuan_bi/buoi-1-chuan-bi.md](./tai_lieu_chuan_bi/buoi-1-chuan-bi.md): Hồ sơ chuẩn bị và tài nguyên trực quan nghiệm thu Buổi 1.

---

## 3. Bản Đồ Phân Hệ Chức Năng (Core Feature Modules)

1. **Task & Urgency Engine:** Quản lý công việc và thuật toán tính điểm khẩn cấp theo hạn chót (Deadline) và thời lượng ước tính.
2. **Google Calendar Sync:** Kết nối và hiển thị sự kiện thời gian biểu cá nhân qua Google Calendar API v3 (OAuth2 Client).
3. **Smart Exam Prep Kit:** Quản lý lịch thi, đồng hồ đếm ngược môn thi sắp tới và cơ chế tự động phân bổ bài ôn thành các task theo ngày.
