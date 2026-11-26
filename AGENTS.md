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
  3. **Smart Exam Prep Kit:** Quản lý kỳ thi, đồng hồ đếm ngược thời gian thực 4 cột trên 1 hàng ngang, thanh tỷ lệ sẵn sàng (`% Readiness`) và bộ tự động phân bổ danh sách chủ đề ôn tập thành các Task theo ngày trước kỳ thi.
- **Ngoài phạm vi (Out-of-Scope):** Không xây dựng Backend riêng, không dùng cơ sở dữ liệu máy chủ (lưu dữ liệu cục bộ bằng `localStorage` và gọi Google Calendar API trực tiếp từ Client), không có tính năng mạng xã hội hay làm việc nhóm.
- **Tech Stack chuẩn:**
  - Nền tảng: **React 18**, **TypeScript 5.x** (Strict mode, không dùng `any`), **Vite 5.x**, Node.js 20 LTS.
  - Giao diện & Design System: **Tailwind CSS 3.4**, **shadcn/ui** (Radix UI primitives), **Lucide React** (`strokeWidth={1.75}`).
  - Dữ liệu, Form & Routing: **TanStack Query v5**, **React Router**, **React Hook Form + Zod**, **`@react-oauth/google`**, **`date-fns`**.
  - Kiểu chữ: **`Fraunces` / `Lora`** (`--font-display`), **`Plus Jakarta Sans` / `Inter`** (`--font-sans`), **`JetBrains Mono`** (`--font-mono`, kèm `tabular-nums`).
  - Triển khai: **GitHub Pages** (bản tĩnh Buổi 2) và **Vercel / Netlify** (ứng dụng hoàn chỉnh Buổi 7).

---

## 3. Quy Định Cấu Trúc Thư Mục (`src/` & `docs/`)

Mọi mã nguồn sau khi khởi tạo dự án Vite + React + TypeScript ở Buổi 4 phải tuân thủ đúng cấu trúc đã định nghĩa trong `README.md` và `docs/folder-structure.md`:

- `src/components/common/`: Các thành phần UI tái sử dụng (`Button`, `Input`, `Skeleton`, `Dialog`, `EmptyState`, `ErrorAlert`).
- `src/components/layout/`: Khung ứng dụng (`ForestSidebar`, `ScenicHeaderBanner`, `PageTransitionWrapper`, `BentoLayout`, `ProtectedRoute`).
- `src/components/task/`: Module công việc (`UrgencyTaskList`, `UrgencyTaskCard`, `TaskFormDialog`).
- `src/components/exam/`: Module lịch thi (`ExamCountdownWidget`, `ExamPrepDrawer`, `ExamCard`, `ExamReadinessBadge`).
- `src/hooks/`: Custom React Hooks (`useCountdownTimer`, `useUrgency`, `useCalendarEvents`).
- `src/lib/`: Hàm thuần túy và tiện ích (`urgency-calculator.ts`, `date-utils.ts`, `exam-task-generator.ts`).
- `src/services/`: Giao tiếp API ngoài và dữ liệu mẫu (`google-calendar-service.ts`, `mockGoogleCalendar.json`).
- `src/types/`: Định nghĩa kiểu dữ liệu TypeScript (`task.ts`, `exam.ts`, `calendar.ts`).

---

## 4. Chuẩn Coding Conventions & Thiết Kế Bắt Buộc

1. **Strict TypeScript:** Tuyệt đối không dùng kiểu `any`. Toàn bộ props, state, dữ liệu `localStorage` và response từ Google Calendar API v3 phải có interface/type rõ ràng trong `src/types/`.
2. **Light-First Cozy Woodland Bento & Chống AI Slop (theo `DESIGN.md`):**
   - Dùng bảng màu khóa: Canvas `--bg-canvas` `#ECE7D8` (Warm Parchment), thanh điều hướng `ForestSidebar` `248px` nền `--bg-sidebar` `#2F4231` (hover `#3B513D`, thẻ mẹo `#4A5F44`, thẻ biển gỗ `#9B6B43` viền `#7A5130` chữ `#FDF9F0`), bề mặt thẻ `--bg-surface-1` `#FAF6EC` (Warm Cream), thẻ con/input/skeleton `--bg-surface-2` `#F1ECDD` (Soft Oat), hover `--bg-surface-hover` `#E7E0CC`, viền mảnh `1px solid #DDD6C3`, focus `#4F7043`.
   - Màu chữ khóa: `--text-primary` `#283629`, `--text-secondary` `#546353`, `--text-muted` `#626F5F`, `--text-on-sidebar` `#F4EFE2`, `--text-on-sidebar-muted` `#C6D1BF`.
   - Màu nhấn & ngữ nghĩa: `--accent-primary` `#4F7043` (hover `#3F5B35`), Success `#2E7D52` (bg `#E3F2EA`), Warning `#B87A1E` (text `#8A580F`, bg `#FBF1DC`), Danger `#B94435` (text `#9A3326`, bg `#FBEAE7`), Info `#3A6F88` (text `#2B566B`, bg `#E5F1F6`).
   - Phân cấp Urgency Tiers chỉ dùng viền mảnh `1px solid` + chấm tròn trạng thái + nhãn pill hữu cơ (`OVERDUE` `#9A3326`/`#F9DFDA`/`#D97C6E`, `EXTREME` `#9E4718`/`#FBE6D4`/`#E09866`, `HIGH` `#8A580F`/`#FBF0D6`/`#D9AE59`, `MODERATE` `#2B566B`/`#E4F0F5`/`#7FAAC0`, `LOW` `#546353`/`#EAE6D9`/`#C9C2AE`). Tuyệt đối không dùng viền trái neon dày, không glassmorphism mờ đục, không drop shadow nặng, không gradient tím-hồng và không nhân vật game trong họa tiết woodland.
   - Bố cục Shell & Bento Grid 12 cột: `ForestSidebar` (`248px`) + `ScenicHeaderBanner` (lời chào, ngày/mùa, đồng hồ, avatar, `+ Thêm Lịch Thi`) + Bento Grid (`Row 1`: `ExamCountdownWidget` 5 cột + `Daily Stats` 7 cột; `Row 2`: `UrgencyTaskList` 8 cột + `Mini Schedule` 4 cột) + `PageTransitionWrapper`.
   - Typography & Số liệu: `--font-display` (`'Fraunces', 'Lora', Georgia, serif`), `--font-sans` (`'Plus Jakarta Sans', 'Inter', -apple-system, sans-serif`), `--font-mono` (`'JetBrains Mono', monospace`) kèm `font-variant-numeric: tabular-nums` (`font-mono tabular-nums`) cho mọi đồng hồ đếm ngược (bắt buộc 4 cột trên 1 hàng ngang), điểm `UrgencyScore`, `% Readiness`, tổng giờ và giờ 24h (`HH:mm`).
   - Spacing & Radius (lưới `4px`): Bento cards & modals `16px` (`rounded-2xl`), nested task cards & inputs `12px` (`rounded-xl`), pills `9999px` (`rounded-full`), badges `8px` (`rounded-lg`).
   - Motion & Trạng thái biên: Chuyển trang `280ms cubic-bezier(0.22, 1, 0.36, 1)`, `ExamPrepDrawer` `360ms cubic-bezier(0.16, 1, 0.3, 1)`, task stagger `35ms/step` (tối đa `210ms`), card hover `-2px`, ambient CSS mây/lá nhẹ trong banner, `@keyframes warmShimmer` cho skeleton, bắt buộc hỗ trợ `@media (prefers-reduced-motion: reduce)`, và xử lý đủ **Warm Shimmer Skeleton**, **Empty State** và **Error Alert**.
   - Form UX: Nhập thời gian định dạng `24h` (`DD/MM/YYYY HH:mm`) và vô hiệu hóa (`disabled`) nút Submit khi dữ liệu form chưa hợp lệ.
   - Accessibility: Tuân thủ WCAG AA/AAA và điều hướng bàn phím (`Tab` / `Enter` / `Escape`).
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
