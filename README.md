# OmniStudy - Smart Exam & Task Planner

[![React](https://img.shields.io/badge/React-18.x-61DAFB?style=flat-square&logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-5.x-646CFF?style=flat-square&logo=vite&logoColor=white)](https://vitejs.dev/)
[![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-3.4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![shadcn/ui](https://img.shields.io/badge/shadcn%2Fui-Radix_UI-000000?style=flat-square&logo=shadcnui&logoColor=white)](https://ui.shadcn.com/)
[![TanStack Query](https://img.shields.io/badge/TanStack_Query-v5-FF4154?style=flat-square&logo=reactquery&logoColor=white)](https://tanstack.com/query/latest)
[![Google Calendar](https://img.shields.io/badge/Google_Calendar_API-v3-4285F4?style=flat-square&logo=googlecalendar&logoColor=white)](https://developers.google.com/calendar)
[![License: MIT](https://img.shields.io/badge/License-MIT-4F46E5?style=flat-square)](./LICENSE)

---

## 1. Giới Thiệu Đề Tài (Project Overview)

**OmniStudy** là nền tảng Web Application quản lý công việc và kế hoạch ôn thi cá nhân theo phong cách **Dark-First Bento Grid**, được thiết kế chuyên biệt cho sinh viên đại học và người tự học có lịch trình bận rộn. 

Ứng dụng giải quyết triệt để vấn đề ôn thi bị động (cramming) và trễ hạn deadline bằng cách kết hợp **Động cơ xếp hạng điểm khẩn cấp theo thời gian thực (Dynamic Urgency Score)** với **Bộ công cụ tự động phân bổ bài ôn trước kỳ thi (Exam Prep Kit)**. Dự án tích hợp trực tiếp với **Google Calendar API** để đồng bộ thời gian biểu cá nhân hai chiều và hỗ trợ lập kế hoạch thời gian (Time-blocking) trực quan.

> Dự án cá nhân được xây dựng và hoàn thiện xuyên suốt khóa đào tạo **Frontend — ProPTIT Web**.

---

## 2. Tính Năng Cốt Lõi (Key Features)

### 2.1. Động Cơ Công Việc & Tính Điểm Deadline (Dynamic Urgency Task Engine)
- **Quản lý công việc trực quan:** Tạo, sửa, xóa, đánh dấu hoàn thành task kèm thời lượng ước tính (`estimatedHours`), hạn chót (`deadline`) và độ ưu tiên gốc (Base Priority từ P1 đến P4).
- **Tính điểm khẩn cấp tự động (Dynamic Urgency Scoring):** Điểm số khẩn cấp $U \in [0, 100]$ tự động tính toán lại theo thời gian thực dựa trên hàm suy giảm phi tuyến tính theo số giờ còn lại:
  $$U = (BasePriority \times 20) + \left(\frac{EstimatedHours}{\max(h, 0.5)} \times 15\right)$$
- **Phân cấp thị giác 5 cấp độ (Urgency Tiers):**
  - `OVERDUE` ($h \le 0$): Viền đỏ rực `#EF4444`, nhãn đỏ, kích hoạt hiệu ứng cảnh báo.
  - `EXTREME` ($U \ge 80$): Viền cam `#F97316`, tự động đẩy lên đầu danh sách.
  - `HIGH` ($60 \le U < 80$): Viền vàng `#EAB308`.
  - `MODERATE` ($35 \le U < 60$): Viền xanh dương `#3B82F6`.
  - `LOW` ($U < 35$): Viền xám `#4B5563`.

### 2.2. Đồng Bộ Thời Gian Biểu Cá Nhân (Google Calendar Integration)
- **Xác thực một chạm Google OAuth 2.0:** Đăng nhập an toàn phía Client bằng `@react-oauth/google`.
- **Tích hợp Google Calendar API v3:** Đọc danh sách sự kiện, lịch học, lịch họp từ Google Calendar (`primary calendar`) và hiển thị trực tiếp lên khung Lịch tuần.
- **Hỗ trợ Time-blocking:** Dễ dàng nhận diện các khoảng thời gian trống trong ngày để ấn định lịch làm việc cho từng công việc.

### 2.3. Quản Lý Lịch Thi & Bộ Sinh Lộ Trình Ôn Tập (Exam Prep Kit)
- **Quản lý kỳ thi:** Nhập tên môn thi, ngày giờ thi, phòng thi và danh sách các chủ đề/chương cần ôn tập.
- **Đồng hồ đếm ngược kỳ thi (Exam Countdown Widget):** Hiển thị thẻ Bento đếm ngược từng ngày, giờ, phút, giây kèm thanh tỷ lệ sẵn sàng (% Readiness).
- **Bộ tự động sinh task ôn tập (Smart Exam Task Generator):** Tự động bóc tách ngày thi và danh sách chủ đề thành các task ôn tập trải đều vào các ngày trước kỳ thi, tự động gán deadline tương ứng.
- **Cảnh báo thi cử đa cấp:** Tự động đổi màu banner cảnh báo khi kỳ thi còn dưới 7 ngày, dưới 72 giờ và dưới 24 giờ.

---

## 3. Công Nghệ Sử Dụng (Tech Stack)

- **Ngôn ngữ & Nền tảng:** TypeScript 5.x, React 18, Vite.
- **Styling & Design System:** Tailwind CSS 3.4, shadcn/ui (Radix UI primitives), Lucide React (`strokeWidth={1.75}`).
- **Quản lý dữ liệu & API ngoài:** 
  - TanStack Query v5 (React Query) quản lý Server State, caching và revalidation.
  - Google Calendar API v3 (REST API) tích hợp qua `@react-oauth/google`.
- **Form UX & Validation:** React Hook Form kết hợp Zod Schema.
- **Xử lý thời gian:** `date-fns` kết hợp font số `JetBrains Mono` với thuộc tính CSS `tabular-nums`.
- **Đóng gói & Triển khai:** Vercel / Netlify.

---

## 4. Lộ Trình Triển Khai Xuyên Suốt 7 Buổi (Curriculum Roadmap)

Dự án được xây dựng tuần tự qua 7 buổi học theo lộ trình đào tạo Frontend:

|    Buổi    | Chủ đề kiến thức          | Đầu việc thực hành trong OmniStudy                                                                            | Output nghiệm thu                                                         |
| :--------: | :------------------------ | :------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------ |
| **Buổi 1** | Kick-off, UI/UX & AI Slop | Chốt đề tài, viết `README.md`, vẽ Wireframe 3 màn kèm 3 trạng thái, phân tích 5 lỗi AI slop, lập bảng tokens. | `README.md`, `DESIGN.md`, `docs/wireframes.md`, `docs/ai-review.md`.      |
| **Buổi 2** | HTML & CSS: Layout        | Dựng layout Bento Grid bằng HTML ngữ nghĩa và CSS thuần, responsive mobile/desktop, kiểm tra a11y.            | 1 màn tĩnh đạt Lighthouse Accessibility $\ge 90$, deploy GitHub Pages.    |
| **Buổi 3** | JS & TypeScript           | Khai báo types (`Task`, `Exam`), viết hàm tính `calculateUrgencyScore` bằng TS, gọi thử API với 3 trạng thái. | Script TypeScript không lỗi type, chạy đúng test case deadline.           |
| **Buổi 4** | React: Components & State | Khởi tạo Vite + React + TS, chuyển layout sang cây component, viết Custom Hook `useCountdownTimer`.           | Cây component hoàn chỉnh, hook đếm ngược không rò rỉ bộ nhớ.              |
| **Buổi 5** | Design System & Form UX   | Cài đặt Tailwind + shadcn/ui, viết Form tạo Task và Form Lịch thi có validate bằng Zod, tạo Shimmer Skeleton. | 2 Form validate đầy đủ, Toast thông báo, Shimmer loading & Empty state.   |
| **Buổi 6** | Routing & Google API      | Thiết lập React Router, cấu hình TanStack Query gọi Google Calendar API, bảo vệ route bằng Protected Route.   | Chuyển trang mượt mà, đồng bộ sự kiện Google Calendar thật lên lịch tuần. |
| **Buổi 7** | Hiệu năng, AI & Ship      | Tối ưu Core Web Vitals, code splitting `React.lazy`, dùng AI sinh component theo `DESIGN.md`, deploy Vercel.  | Link deploy Vercel chính thức, báo cáo Lighthouse, video demo 3 phút.     |

---

## 5. Quy Chuẩn Giao Diện (Design System & Tokens)

Dự án áp dụng ngôn ngữ thiết kế **Dark-First Bento Grid**:
- **Màu nền Canvas:** `#090A0F`
- **Màu thẻ Bento (Surface 1):** `#11131A`
- **Màu thẻ con (Surface 2):** `#181B24`
- **Viền mảnh phân tách:** `1px` màu `#222634`
- **Màu chủ đạo (Primary Accent):** Indigo 500 `#6366F1`
- **Màu ngữ nghĩa:** Success `#10B981`, Warning `#F59E0B`, Danger `#EF4444`, Info `#3B82F6`
- **Font chữ:**
  - Giao diện: `'Inter'`, sans-serif.
  - Số đếm & Đồng hồ: `'JetBrains Mono'`, monospace (bắt buộc bật `font-variant-numeric: tabular-nums`).
- **Khoảng cách & Bo góc:** Thang khoảng cách 4px/8px, bo góc thẻ `8px` (`rounded-lg`) và khối Bento `12px` (`rounded-xl`).

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
│   ├── ai-review.md                # Báo cáo phân tích 5 lỗi AI Slop và cách khắc phục
│   └── roadmap.md                  # Chi tiết lộ trình đào tạo và bài tập 7 buổi
├── src/
│   ├── components/                 # Các component React phân tách theo module
│   │   ├── common/                 # Button, Input, Skeleton, Dialog, EmptyState
│   │   ├── exam/                   # ExamCountdownWidget, ExamPrepDrawer, ExamCard
│   │   ├── layout/                 # TopHeader, BentoLayout, ProtectedRoute
│   │   └── task/                   # UrgencyTaskList, UrgencyTaskCard, TaskFormDialog
│   ├── hooks/                      # Custom React Hooks (useCountdown, useUrgency)
│   ├── lib/                        # Hàm tiện ích thuần túy (urgency-calculator.ts, date-utils.ts)
│   ├── services/                   # Tích hợp API (google-calendar-service.ts)
│   ├── types/                      # Định nghĩa kiểu dữ liệu TypeScript (task.ts, exam.ts)
│   ├── App.tsx                     # Khởi tạo React Router và bố cục chính
│   ├── main.tsx                    # Điểm khởi chạy ứng dụng, bọc QueryClientProvider
│   └── index.css                   # Cấu hình Tailwind CSS và Design Tokens
├── DESIGN.md                       # Tài liệu đặc tả Design Tokens và nguyên tắc chống AI Slop
├── package.json                    # Khai báo dependencies và scripts
├── tailwind.config.ts              # Cấu hình chủ đề Tailwind CSS Dark Mode
└── tsconfig.json                   # Cấu hình kiểm tra kiểu TypeScript
```

---

## 8. Tác Giả & Bản Quyền

- **Học viên:** Nguyễn Quốc Khánh
- **Khóa học:** Frontend Development 2026 — ProPTIT Web
- **Giấy phép:** [MIT License](./LICENSE)
