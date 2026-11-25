# Quy Tắc Kiến Trúc & Phát Triển Dành Cho AI Agent (AGENTS.md) - OmniStudy

Tài liệu này định nghĩa bộ quy tắc bắt buộc dành cho AI Agent khi tham gia khảo sát, thiết kế, hướng dẫn lập trình, kiểm thử và bảo trì mã nguồn trong dự án **OmniStudy (Smart Exam & Task Planner)** — đồ án cá nhân xuyên suốt khóa đào tạo **Frontend Development 2026 — ProPTIT Web**.

---

## 1. Quy Tắc Bảo Vệ Tài Liệu Thiết Kế (`docs/` & `DESIGN.md`)

1. **Tôn trọng tài liệu đặc tả gốc:** Các tệp đặc tả cốt lõi gồm `README.md`, `DESIGN.md`, `docs/product.md`, `docs/spec.md`, `docs/roadmap.md`, `docs/wireframes.md`, `docs/ai-review.md` là nguồn chân lý duy nhất về phạm vi sản phẩm và lộ trình thực thi.
2. **Không tự ý sửa đổi ngoài yêu cầu:** Không xóa, đổi tên hoặc thay đổi nội dung kiến trúc trong `docs/` và `DESIGN.md` nếu không có yêu cầu trực tiếp từ người dùng.
3. **Tra cứu trước khi hướng dẫn hoặc viết code:** Trước khi thực hiện bất kỳ nội dung nào từ **Buổi 1 đến Buổi 7**, Agent bắt buộc phải đối chiếu phạm vi trong `docs/product.md`, công thức nghiệp vụ trong `docs/spec.md`, quy chuẩn giao diện trong `DESIGN.md` và tiêu chuẩn nghiệm thu trong `docs/roadmap.md`.

---

## 2. Tóm Tắt Phạm Vi & Tech Stack Cốt Lõi

- **Phạm vi sản phẩm (3 phân hệ cốt lõi):**
  1. **Task & Urgency Engine:** Tạo/sửa/xóa/hoàn thành công việc, tự động tính điểm khẩn cấp động $U \in [0, 100]$ theo hạn chót và thời lượng ước tính, phân loại 5 cấp bậc thị giác (`OVERDUE`, `EXTREME`, `HIGH`, `MODERATE`, `LOW`).
  2. **Google Calendar Sync:** Đăng nhập Google OAuth 2.0 phía Client (`@react-oauth/google`), đọc sự kiện từ Google Calendar API v3 (`primary calendar`), hỗ trợ Time-blocking và có cơ chế fallback bằng dữ liệu mẫu (`mockGoogleCalendar.json`).
  3. **Smart Exam Prep Kit:** Quản lý kỳ thi, đồng hồ đếm ngược thời gian thực, thanh tỷ lệ sẵn sàng (`% Readiness`) và bộ tự động phân bổ danh sách chủ đề ôn tập thành các Task theo ngày trước kỳ thi.
- **Ngoài phạm vi (Out-of-Scope):** Không xây dựng Backend riêng, không dùng cơ sở dữ liệu máy chủ (lưu dữ liệu cục bộ bằng `localStorage` và gọi Google Calendar API trực tiếp từ Client), không có tính năng mạng xã hội hay làm việc nhóm.
- **Tech Stack chuẩn:**
  - Nền tảng: **React 18**, **TypeScript 5.x** (Strict mode, không dùng `any`), **Vite 5.x**, Node.js 20 LTS.
  - Giao diện & Design System: **Tailwind CSS 3.4**, **shadcn/ui** (Radix UI primitives), **Lucide React** (`strokeWidth={1.75}`).
  - Dữ liệu, Form & Routing: **TanStack Query v5**, **React Router**, **React Hook Form + Zod**, **`@react-oauth/google`**, **`date-fns`**.
  - Triển khai: **GitHub Pages** (bản tĩnh Buổi 2) và **Vercel / Netlify** (ứng dụng hoàn chỉnh Buổi 7).

---

## 3. Quy Định Cấu Trúc Thư Mục (`src/` & `docs/`)

Mọi mã nguồn sau khi khởi tạo dự án Vite + React + TypeScript ở Buổi 4 phải tuân thủ đúng cấu trúc đã định nghĩa trong `README.md` và `docs/folder-structure.md`:

- `src/components/common/`: Các thành phần UI tái sử dụng (`Button`, `Input`, `Skeleton`, `Dialog`, `EmptyState`, `ErrorAlert`).
- `src/components/layout/`: Khung ứng dụng (`TopHeader`, `BentoLayout`, `ProtectedRoute`).
- `src/components/task/`: Module công việc (`UrgencyTaskList`, `UrgencyTaskCard`, `TaskFormDialog`).
- `src/components/exam/`: Module lịch thi (`ExamCountdownWidget`, `ExamPrepDrawer`, `ExamCard`, `ExamReadinessBadge`).
- `src/hooks/`: Custom React Hooks (`useCountdownTimer`, `useUrgency`).
- `src/lib/`: Hàm thuần túy và tiện ích (`urgency-calculator.ts`, `date-utils.ts`, `exam-task-generator.ts`).
- `src/services/`: Giao tiếp API ngoài và dữ liệu mẫu (`google-calendar-service.ts`, `mockGoogleCalendar.json`).
- `src/types/`: Định nghĩa kiểu dữ liệu TypeScript (`task.ts`, `exam.ts`, `calendar.ts`).

---

## 4. Chuẩn Coding Conventions & Thiết Kế Bắt Buộc

1. **Strict TypeScript:** Tuyệt đối không dùng kiểu `any`. Toàn bộ props, state, dữ liệu `localStorage` và response từ Google Calendar API v3 phải có interface/type rõ ràng trong `src/types/`.
2. **Dark-First Bento Grid & Chống AI Slop (theo `DESIGN.md`):**
   - Dùng nền Canvas `#090A0F`, bề mặt thẻ `#11131A` / `#181B24`, viền mảnh `1px solid #222634`, màu nhấn Indigo 500 `#6366F1`. Tuyệt đối không dùng glassmorphism mờ mịt, drop shadow nặng hoặc gradient tím-hồng tùy tiện.
   - Bắt buộc dùng `'JetBrains Mono'` kèm `font-variant-numeric: tabular-nums` (`font-mono tabular-nums`) cho mọi đồng hồ đếm ngược, số giờ còn lại và điểm `UrgencyScore`.
   - Mọi thành phần hiển thị dữ liệu phải xử lý đủ 3 trạng thái biên: **Loading (Shimmer Skeleton)**, **Empty State** (có nút hành động chính) và **Error Alert** (có nút thử lại hoặc đăng nhập lại Google).
   - Tuân thủ thang khoảng cách bội số `4px` và chuẩn Accessibility (WCAG AA/AAA, điều hướng bàn phím `Tab` / `Enter` / `Escape`).
3. **Hiệu năng React & Quản lý State:**
   - Cô lập state của đồng hồ đếm ngược trong component/hook riêng để không gây re-render toàn bộ bảng Task mỗi giây.
   - Luôn dọn dẹp `setInterval` / listener trong cleanup function của `useEffect`.
   - Áp dụng `React.lazy` và `Suspense` cho route `/calendar` và modal nặng ở Buổi 7.

---

## 5. Quy Tắc Đào Tạo & Hỗ Trợ Thực Hành (Hands-On Learning Rule)

Mục đích của dự án OmniStudy là giúp học viên nắm vững nền tảng HTML/CSS, TypeScript, React, quản lý State, Design System và quy trình ship sản phẩm Frontend thực tế qua 7 buổi học. Do đó:

1. **Ưu tiên giải thích bản chất kỹ thuật:** Khi hướng dẫn làm bài hoặc sửa lỗi, luôn giải thích rõ cơ chế hoạt động (CSS Grid vs Flexbox, Immutability, Closure/Cleanup trong `useEffect`, Zod Schema, Caching trong TanStack Query, luồng OAuth 2.0...).
2. **Không viết tắt hoặc để lại `// TODO`:** Mọi hướng dẫn hoặc đoạn mã mẫu cần rõ ràng, đúng chuẩn TypeScript và bám sát từng chặng trong `docs/roadmap.md`.
3. **Kiểm chứng sau mỗi thay đổi:** Sau khi chỉnh sửa mã nguồn, luôn kiểm tra bằng `npx tsc --noEmit`, `npm run lint`, `npm run build` hoặc kiểm tra trực tiếp trên trình duyệt (DevTools, React DevTools, Lighthouse).
4. **Quy tắc văn phong & Git:** Không dùng icon hoặc emoji trong mã nguồn, log, tài liệu kỹ thuật và câu trả lời. Chỉ `git commit` và `git push` khi có yêu cầu trực tiếp từ người dùng.
