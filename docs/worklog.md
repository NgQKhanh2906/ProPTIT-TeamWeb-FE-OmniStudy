# Project Execution Worklog - OmniStudy

Bảng theo dõi tiến độ thực thi dự án **OmniStudy (Smart Exam & Task Planner)** bám sát lộ trình 7 buổi học trong [roadmap.md](./roadmap.md).

## 1. Quy Ước Trạng Thái (Status Legend)
- `TODO`: Chưa bắt đầu thực hiện.
- `IN_PROGRESS`: Đang trong quá trình thực hiện và kiểm thử.
- `DONE`: Đã hoàn thành, đạt toàn bộ tiêu chuẩn nghiệm thu (Deliverables) và hợp nhất vào nhánh `main`.

---

## 2. Bảng Theo Dõi Tiến Độ 7 Buổi Học (Curriculum Tracker)

| Buổi | Chủ Đề & Đầu Việc Chính | Trạng Thái | Ngày Cập Nhật | Tài Liệu / Output Nghiệm Thu & Ghi Chú Kỹ Thuật |
| :---: | :--- | :---: | :---: | :--- |
| **Buổi 1** | Chốt đề tài OmniStudy, vẽ User Flow & Wireframe 3 màn đủ 3 trạng thái, phân tích 5 lỗi AI Slop, lập bảng Design Tokens | `DONE` | 2026-11-25 | Hoàn thành qua PR `#1`: `README.md`, `DESIGN.md`, `docs/wireframes.md`, `docs/ai-review.md`, `docs/tai_lieu_chuan_bi/buoi-1-chuan-bi.md` và bộ SVG trong `docs/assets/buoi-1/`. |
| **Buổi 2** | HTML5 ngữ nghĩa & CSS Bento Grid 12 cột responsive, `:focus-visible`, kiểm tra Lighthouse Accessibility $\ge 90$, deploy tĩnh | `TODO` | - | Trọng tâm: Flexbox/CSS Grid, CSS Variables từ `DESIGN.md`, điều hướng bàn phím `Tab`. |
| **Buổi 3** | Khai báo TypeScript interfaces (`Task`, `UrgencyTier`, `Exam`, `GoogleCalendarEvent`), viết `calculateUrgencyScore` và thử nghiệm API với 3 trạng thái | `TODO` | - | Trọng tâm: Strict TypeScript (không dùng `any`), Pure Functions, phân tích HTTP trên DevTools Network. |
| **Buổi 4** | Khởi tạo Vite + React + TypeScript, tách cây Component, quản lý State danh sách Task, viết hook `useCountdownTimer` có cleanup | `TODO` | - | Trọng tâm: Lifting State Up, tránh rò rỉ bộ nhớ `setInterval`, xử lý re-render thừa bằng React DevTools. |
| **Buổi 5** | Tích hợp Tailwind CSS + shadcn/ui, xây dựng Form Tạo Task & Form Lịch Thi với React Hook Form + Zod, Shimmer Skeleton & Empty State | `TODO` | - | Trọng tâm: Validate trường dữ liệu, chống double submit, đồng bộ Design Tokens vào `tailwind.config.ts`. |
| **Buổi 6** | Thiết lập React Router (`/`, `/calendar`, `/exams/:id`, `/login`), TanStack Query v5 đồng bộ Google Calendar API v3, `@react-oauth/google` & `ProtectedRoute` | `TODO` | - | Trọng tâm: Caching Server State, xử lý lỗi 401 và chế độ fallback `mockGoogleCalendar.json`. |
| **Buổi 7** | Code splitting (`React.lazy` & `Suspense`), tối ưu CLS với `tabular-nums`, review AI component `ExamReadinessBadge`, deploy Vercel/Netlify & quay video demo | `TODO` | - | Trọng tâm: Core Web Vitals, Performance $\ge 85$, cấu hình `VITE_GOOGLE_CLIENT_ID` trên Production. |

---

## 3. Nhật Ký Quyết Định Kỹ Thuật (Technical Decision Log)

| Ngày | Hạng Mục | Quyết Định & Lý Do |
| :---: | :--- | :--- |
| 2026-11-25 | Chốt phạm vi kiến trúc OmniStudy | Chốt mô hình Client-side SPA (React 18 + TypeScript + Vite + Client-side Storage + Google Calendar API v3), loại bỏ toàn bộ các thư mục, cấu hình và tài liệu dư thừa từ khung dự án cũ để tập trung hoàn toàn vào 3 phân hệ cốt lõi của OmniStudy. |
| 2026-11-26 | Tái thiết kế sang Light-First Cozy Woodland Bento & đồng bộ 7 điểm nhất quán | Chuyển toàn bộ Design System từ Dark-First sang **Light-First Cozy Woodland Bento** (Canvas `#ECE7D8`, Forest Sidebar `#2F4231`, Wood Sign `#9B6B43`, Surface `#FAF6EC`/`#F1ECDD`, Pine Moss `#4F7043`, viền 1px `#DDD6C3`, Font `Fraunces` + `Plus Jakarta Sans` + `JetBrains Mono`) kèm 26 swatches và 6 SVG minh họa mới; khắc phục đồng bộ 7 lỗi kiểm duyệt: (1) Lịch tháng 11/2026 chuẩn (`Thứ Tư, 25/11/2026`, tuần `23/11-29/11`, thi `08:30 Thứ Bảy, 28/11/2026`); (2) Readiness `60% (3/5 chủ đề)` khớp đúng 3 mục đã xong và 2 mục còn lại; (3) Đồng hồ đếm ngược 1 hàng ngang 4 ô không rớt chữ `Giây`; (4) Bắt buộc `tabular-nums` trên 100% số liệu; (5) Loading Skeleton dùng `@keyframes warmShimmer`; (6) Dialog Modals dùng chuẩn 24h (`HH:mm DD/MM/YYYY`) và khóa nút Submit (`disabled`) khi dữ liệu sai; (7) Tổng giờ Daily Stats `8.5h` khớp tổng 4 task (`1.5h + 2.0h + 3.0h + 2.0h`). |
