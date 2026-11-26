# OmniStudy - Smart Exam & Task Planner

[![React](https://img.shields.io/badge/React-18.x-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-5.x-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-3.4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-Radix_UI-2F4231?style=flat-square&logo=shadcnui&logoColor=white)](https://ui.shadcn.com/)
[![TanStack Query](https://img.shields.io/badge/TanStack_Query-v5-FF4154?style=flat-square&logo=reactquery&logoColor=white)](https://tanstack.com/query/latest)
[![Google Calendar](https://img.shields.io/badge/Google_Calendar_API-v3-3A6F88?style=flat-square&logo=googlecalendar&logoColor=white)](https://developers.google.com/calendar)
[![License: MIT](https://img.shields.io/badge/License-MIT-4F7043?style=flat-square)](./LICENSE)

---

## 1. Giới Thiệu Đề Tài (Project Overview)

**OmniStudy** là nền tảng Web Application quản lý công việc và kế hoạch ôn thi cá nhân theo phong cách **Light-First Cozy Woodland Bento Grid** (cảm hứng rừng thông và mặt hồ tĩnh lặng với họa tiết lá cây tối giản, tuyệt đối không dùng nhân vật game), được thiết kế chuyên biệt cho sinh viên đại học và người tự học có lịch trình bận rộn.

Ứng dụng giải quyết triệt để vấn đề ôn thi bị động (cramming) và trễ hạn deadline bằng cách kết hợp **Động cơ xếp hạng điểm khẩn cấp theo thời gian thực (Dynamic Urgency Score)** với **Bộ công cụ tự động phân bổ bài ôn trước kỳ thi (Exam Prep Kit)**. Dự án tích hợp trực tiếp với **Google Calendar API v3** để đồng bộ thời gian biểu cá nhân và hỗ trợ lập kế hoạch thời gian (Time-blocking) trực quan.

> Dự án cá nhân được xây dựng và hoàn thiện xuyên suốt khóa đào tạo **Frontend Development 2026 — ProPTIT Web**.

---

## 2. Tính Năng Cốt Lõi (Key Features)

### 2.1. Động Cơ Công Việc & Tính Điểm Deadline (Dynamic Urgency Task Engine)
- **Quản lý công việc trực quan:** Tạo, sửa, xóa, đánh dấu hoàn thành task kèm thời lượng ước tính (`estimatedHours`), hạn chót (`deadline` theo định dạng `24h`) và độ ưu tiên gốc (`basePriority` từ 1 đến 4).
- **Tính điểm khẩn cấp tự động (Dynamic Urgency Scoring):** Điểm số khẩn cấp $U \in [0, 100]$ tự động tính toán lại theo thời gian thực dựa trên hàm suy giảm phi tuyến tính theo số giờ còn lại $h$:
  $$U = (BasePriority \times 20) + \left(\frac{EstimatedHours}{\max(h, 0.5)} \times 15\right)$$
- **Phân cấp thị giác 5 cấp độ (Urgency Tiers):** Hiển thị bằng **viền mảnh `1px solid` + chấm tròn trạng thái + nhãn pill hữu cơ** (không dùng viền trái neon dày):
  - `OVERDUE` ($h \le 0$): Chữ `#9A3326`, nền pill `#F9DFDA`, viền `#D97C6E` (kèm hiệu ứng nhịp thở nhẹ).
  - `EXTREME` ($U \ge 80$): Chữ `#9E4718`, nền pill `#FBE6D4`, viền `#E09866`, tự động đẩy lên đầu danh sách.
  - `HIGH` ($60 \le U < 80$): Chữ `#8A580F`, nền pill `#FBF0D6`, viền `#D9AE59`.
  - `MODERATE` ($35 \le U < 60$): Chữ `#2B566B`, nền pill `#E4F0F5`, viền `#7FAAC0`.
  - `LOW` ($U < 35$): Chữ `#546353`, nền pill `#EAE6D9`, viền `#C9C2AE`.

### 2.2. Đồng Bộ Thời Gian Biểu Cá Nhân (Google Calendar Integration)
- **Xác thực một chạm Google OAuth 2.0:** Đăng nhập an toàn phía Client bằng `@react-oauth/google`.
- **Tích hợp Google Calendar API v3:** Đọc danh sách sự kiện, lịch học, lịch họp từ Google Calendar (`primary calendar`) và hiển thị lên Lịch tuần (`Thứ 2 - CN`) với tông màu thông tin dịu `--semantic-info` (`#3A6F88` / nền `#E5F1F6`, chữ `#2B566B`).
- **Hỗ trợ Time-blocking & Chế độ Dự phòng:** Nhận diện nhanh khoảng thời gian trống trong ngày để ấn định lịch làm việc cho từng task; tự động fallback sang `mockGoogleCalendar.json` khi mất mạng hoặc chưa đăng nhập Google.

### 2.3. Quản Lý Lịch Thi & Bộ Sinh Lộ Trình Ôn Tập (Exam Prep Kit)
- **Quản lý kỳ thi:** Nhập tên môn thi, ngày giờ thi (`24h`), phòng thi, trọng số điểm và danh sách các chủ đề cần ôn tập.
- **Đồng hồ đếm ngược kỳ thi (`ExamCountdownWidget`):** Hiển thị đếm ngược 4 cột trên 1 hàng ngang duy nhất (`Ngày | Giờ | Phút | Giây`) bằng `JetBrains Mono` (`tabular-nums`) kèm thanh tỷ lệ sẵn sàng (`% Readiness` màu `#2E7D52`).
- **Bộ tự động sinh task ôn tập (`ExamPrepDrawer`):** Bóc tách ngày thi và các chủ đề chưa hoàn thành thành các task ôn tập trải đều vào những ngày trước kỳ thi, tự động đẩy lên Dashboard và Lịch tuần.
- **Cảnh báo thi cử đa cấp:** Tự động chuyển sắc thái cảnh báo dịu khi kỳ thi còn dưới 7 ngày (`#B87A1E` / `#FBF1DC`), dưới 72 giờ và dưới 24 giờ (`#B94435` / `#FBEAE7`).

---

## 3. Công Nghệ Sử Dụng (Tech Stack)

- **Ngôn ngữ & Nền tảng:** TypeScript 5.x (Strict mode, không dùng `any`), React 18, Vite 5.x.
- **Styling & Design System:** Tailwind CSS 3.4, shadcn/ui (Radix UI primitives), Lucide React (`strokeWidth={1.75}`).
- **Quản lý dữ liệu & API ngoài:**
  - TanStack Query v5 (React Query) quản lý Server State, caching và revalidation.
  - Google Calendar API v3 (REST API) tích hợp qua `@react-oauth/google`.
- **Form UX & Validation:** React Hook Form kết hợp Zod Schema (định dạng thời gian `24h`, khóa nút submit khi dữ liệu không hợp lệ).
- **Xử lý thời gian & Typography:** `date-fns` kết hợp `--font-display` (`Fraunces`, `Lora`), `--font-sans` (`Plus Jakarta Sans`, `Inter`) và `--font-mono` (`JetBrains Mono` với `font-variant-numeric: tabular-nums`).
- **Đóng gói & Triển khai:** GitHub Pages (bản tĩnh Buổi 2) và Vercel / Netlify (ứng dụng hoàn chỉnh Buổi 7).

---

## 4. Lộ Trình Triển Khai Xuyên Suốt 7 Buổi (Curriculum Roadmap)

Dự án được xây dựng tuần tự qua 7 buổi học theo lộ trình đào tạo Frontend:

| Buổi | Chủ đề kiến thức | Đầu việc thực hành trong OmniStudy | Output nghiệm thu |
| :---: | :--- | :--- | :--- |
| **Buổi 1** | Kick-off, UI/UX & AI Slop | Chốt đề tài, viết `README.md`, vẽ Wireframe 3 màn kèm 3 trạng thái, phân tích lỗi AI Slop, lập bảng tokens Cozy Woodland Bento. | `README.md`, `DESIGN.md`, `docs/wireframes.md`, `docs/ai-review.md`. |
| **Buổi 2** | HTML & CSS: Layout | Dựng `ForestSidebar` (`248px`), `ScenicHeaderBanner` và Bento Grid 12 cột bằng HTML ngữ nghĩa + CSS thuần, responsive, a11y. | 1 màn tĩnh đạt Lighthouse Accessibility $\ge 90$, deploy GitHub Pages. |
| **Buổi 3** | JS & TypeScript | Khai báo types (`Task`, `Exam`), viết `calculateUrgencyScore`, `examTaskGenerator` bằng TS Strict, xử lý 3 trạng thái fetch API. | Script TypeScript không lỗi type, chạy đúng test case deadline. |
| **Buổi 4** | React: Components & State | Khởi tạo Vite + React + TS, chuyển layout sang cây component (`BentoLayout`, `ForestSidebar`, `ScenicHeaderBanner`), viết `useCountdownTimer`. | Cây component hoàn chỉnh, hook đếm ngược không rò rỉ bộ nhớ. |
| **Buổi 5** | Design System & Form UX | Cài Tailwind + shadcn/ui theo tokens `#ECE7D8`/`#2F4231`/`#FAF6EC`, viết Form Zod (`24h`, khóa submit khi lỗi), dựng Warm Shimmer Skeleton. | 2 Form validate đầy đủ, Toast thông báo, Shimmer loading & Empty state. |
| **Buổi 6** | Routing & Google API | Thiết lập React Router bọc trong `PageTransitionWrapper`, cấu hình TanStack Query gọi Google Calendar API, `ProtectedRoute`. | Chuyển trang mượt mà (`280ms`), đồng bộ sự kiện Google Calendar lên lịch tuần. |
| **Buổi 7** | Hiệu năng, AI & Ship | Code splitting `React.lazy`, kiểm tra `CLS = 0`, `prefers-reduced-motion`, sinh `ExamReadinessBadge` theo `DESIGN.md`, deploy Vercel. | Link deploy Vercel chính thức, báo cáo Lighthouse, video demo 3 phút. |

---

## 5. Quy Chuẩn Giao Diện, Bố Cục & Chuyển Động (Design System & Shell Spec)

Dự án áp dụng ngôn ngữ thiết kế **Light-First Cozy Woodland Bento**:
- **Hệ màu nền & thanh điều hướng:**
  - App Canvas (`--bg-canvas`): `#ECE7D8` (Warm Parchment)
  - Sidebar (`--bg-sidebar`): `#2F4231` (Deep Forest Pine, rộng `248px`, hover `#3B513D`, thẻ mẹo `#4A5F44`, thẻ biển gỗ `#9B6B43` viền `#7A5130` chữ `#FDF9F0`)
  - Surface 1 (`--bg-surface-1`): `#FAF6EC` (Warm Cream cho Bento cards, drawer, modal)
  - Surface 2 (`--bg-surface-2`): `#F1ECDD` (Soft Oat cho task con, input, skeleton) và Hover (`--bg-surface-hover`): `#E7E0CC`
  - Viền phân tách (`--border-subtle`): `1px solid #DDD6C3`, viền Focus (`--border-focus`): `#4F7043`
- **Hệ màu chữ & thương hiệu:**
  - Chữ chính `--text-primary`: `#283629`, phụ `--text-secondary`: `#546353`, mờ `--text-muted`: `#626F5F`; chữ trên sidebar `--text-on-sidebar`: `#F4EFE2`, `--text-on-sidebar-muted`: `#C6D1BF`
  - Màu nhấn `--accent-primary`: `#4F7043` (hover `#3F5B35`)
  - Ngữ nghĩa: Success `#2E7D52` (bg `#E3F2EA`), Warning `#B87A1E` (text `#8A580F`, bg `#FBF1DC`), Danger `#B94435` (text `#9A3326`, bg `#FBEAE7`), Info `#3A6F88` (text `#2B566B`, bg `#E5F1F6`)
- **Kiểu chữ & Thang khoảng cách (4px grid):**
  - `--font-display`: `'Fraunces', 'Lora', Georgia, serif`
  - `--font-sans`: `'Plus Jakarta Sans', 'Inter', -apple-system, sans-serif`
  - `--font-mono`: `'JetBrains Mono', monospace` (bắt buộc `font-variant-numeric: tabular-nums` trên mọi con số)
  - Bo góc: Bento cards & modals `16px` (`rounded-2xl`), nested task cards & inputs `12px` (`rounded-xl`), pills `9999px` (`rounded-full`), badges `8px` (`rounded-lg`).
- **Bố cục Shell & Chuyển động:**
  - Khung ứng dụng gồm: `ForestSidebar` (`248px` bên trái) + `ScenicHeaderBanner` (lời chào, ngày/mùa, đồng hồ 24h, avatar, nút `+ Thêm Lịch Thi`) + Bento Grid 12 cột (`Row 1`: `ExamCountdownWidget` 5 cột + `Daily Stats` 7 cột; `Row 2`: `UrgencyTaskList` 8 cột + `Mini Schedule` 4 cột) + `PageTransitionWrapper`.
  - Chuyển động chuẩn: chuyển trang `280ms cubic-bezier(0.22, 1, 0.36, 1)`, `ExamPrepDrawer` trượt `360ms cubic-bezier(0.16, 1, 0.3, 1)`, stagger danh sách task `35ms/step` (tối đa `210ms`), card hover `-2px`, chuyển động CSS nhẹ nhàng trong banner, `@keyframes warmShimmer` cho skeleton, và bắt buộc hỗ trợ `@media (prefers-reduced-motion: reduce)`.

*Chi tiết toàn bộ quy chuẩn thiết kế và hướng dẫn kiểm soát AI slop được lưu trữ tại [DESIGN.md](./DESIGN.md).*

---

## 6. Hướng Dẫn Cài Đặt & Chạy Môi Trường Phát Triển (Quick Start)

### Yêu cầu môi trường
- Node.js version 20 LTS trở lên
- Trình quản lý gói `npm` hoặc `pnpm`

### Các bước khởi chạy
1. Clone repository về máy:
   ```bash
   git clone https://github.com/your-username/omnistudy.git
   cd omnistudy
   ```
2. Cài đặt các gói phụ thuộc:
   ```bash
   npm install
   ```
3. Tạo file biến môi trường `.env.local` từ mẫu:
   ```bash
   cp .env.example .env.local
   ```
   *Cập nhật `VITE_GOOGLE_CLIENT_ID` của bạn từ Google Cloud Console.*
4. Chạy máy chủ phát triển:
   ```bash
   npm run dev
   ```
5. Mở trình duyệt tại địa chỉ: `http://localhost:5173`

---

## 7. Cấu Trúc Thư Mục Dự Án (Project Structure)

```text
omnistudy/
├── docs/
│   ├── wireframes.md               # Bản đặc tả Wireframe 3 màn chính kèm các trạng thái
│   ├── ai-review.md                # Báo cáo phân tích lỗi AI Slop và giải pháp Cozy Woodland Bento
│   └── roadmap.md                  # Chi tiết lộ trình đào tạo và bài tập 7 buổi
├── src/
│   ├── components/                 # Các component React phân tách theo module
│   │   ├── common/                 # Button, Input, Skeleton, Dialog, EmptyState, ErrorAlert
│   │   ├── exam/                   # ExamCountdownWidget, ExamPrepDrawer, ExamCard, ExamReadinessBadge
│   │   ├── layout/                 # ForestSidebar, ScenicHeaderBanner, PageTransitionWrapper, BentoLayout, ProtectedRoute
│   │   └── task/                   # UrgencyTaskList, UrgencyTaskCard, TaskFormDialog
│   ├── hooks/                      # Custom React Hooks (useCountdownTimer, useUrgency, useCalendarEvents)
│   ├── lib/                        # Hàm tiện ích thuần túy (urgency-calculator.ts, exam-task-generator.ts, date-utils.ts)
│   ├── services/                   # Tích hợp API (google-calendar-service.ts, mockGoogleCalendar.json)
│   ├── types/                      # Định nghĩa kiểu dữ liệu TypeScript (task.ts, exam.ts, calendar.ts)
│   ├── App.tsx                     # Khởi tạo React Router, PageTransitionWrapper và Code Splitting
│   ├── main.tsx                    # Điểm khởi chạy ứng dụng, bọc QueryClientProvider
│   └── index.css                   # Khai báo Tailwind CSS, Design Tokens và keyframes shimmer/motion
├── DESIGN.md                       # Tài liệu đặc tả Design Tokens và nguyên tắc chống AI Slop
├── package.json                    # Khai báo dependencies và scripts
├── tailwind.config.ts              # Cấu hình chủ đề Tailwind CSS Cozy Woodland Bento
└── tsconfig.json                   # Cấu hình kiểm tra kiểu TypeScript
```

---

## 8. Tác Giả & Bản Quyền

- **Học viên:** Nguyễn Quốc Khánh
- **Khóa học:** Frontend Development 2026 — ProPTIT Web
- **Giấy phép:** [MIT License](./LICENSE)
