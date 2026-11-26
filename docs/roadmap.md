# Execution Roadmap - OmniStudy (Frontend 2026)

Lộ trình phát triển dự án **OmniStudy** gồm **7 chặng thực hành tuần tự** tương ứng với 7 buổi học của chương trình đào tạo Frontend. Mỗi chặng được thiết kế theo mô hình: **Học lý thuyết trên lớp -> Thực hành tính năng cụ thể vào OmniStudy -> Tự kiểm tra và nộp bài.**

---

## Buổi 1: Chốt Đề Tài, Tư Duy UI/UX & Bộ Quy Chuẩn Thiết Kế

- **Mục tiêu buổi học:** Hiểu cách thức hoạt động của Web; nắm vững UX Heuristics (NN/g), Visual Hierarchy, Spacing thang `4px`; biết nhận diện lỗi AI Slop; chốt đề tài và lập bảng tokens.
- **Đầu việc thực hành trên OmniStudy:**
  1. Khởi tạo repository GitHub `omnistudy`, tạo file `README.md` với mô tả 3-5 câu về bài toán giải quyết cho sinh viên ôn thi.
  2. Vẽ Wireframe 3 màn chính (Focus Dashboard, Weekly Calendar, Exam Prep Drawer) kèm đủ 3 trạng thái biên (**Warm Shimmer Skeleton**, **Empty State**, **Error Alert**), chốt dữ liệu lịch nhất quán (`Thứ Tư, 25/11/2026`, tuần `Thứ 2 (23/11) - CN (29/11)`, kỳ thi `08:30 - Thứ Bảy, 28/11/2026`, `Readiness 60% = 3/5`, tổng giờ ngày `8.5h`).
  3. Dùng công cụ AI sinh thử giao diện Dashboard, chỉ ra ít nhất 5 lỗi AI Slop (nền tối/neon/glassmorphism lạm dụng, thiếu `tabular-nums`, xuống dòng đồng hồ đếm ngược, thiếu 3 trạng thái biên, sai lệch số liệu/validation) trong `docs/ai-review.md` và đối chiếu với giải pháp **Light-First Cozy Woodland Bento**.
  4. Lập bảng Tokens và quy tắc AI trong `DESIGN.md`: Canvas `#ECE7D8`, `ForestSidebar` `#2F4231` (`248px`), Surface-1 `#FAF6EC`, Surface-2 `#F1ECDD`, Viền `#DDD6C3`, Accent `#4F7043`, Font `Fraunces` / `Plus Jakarta Sans` / `JetBrains Mono` (`tabular-nums`).
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - Repo GitHub có file `README.md` mô tả đúng chuẩn.
  - File `DESIGN.md` lưu đầy đủ bảng tokens, 26 swatch links, spec chuyển động và quy tắc chống AI slop.
  - File `docs/wireframes.md` chứa luồng và bản vẽ 3 màn hình nhất quán dữ liệu.
  - File `docs/ai-review.md` chứa phân tích 5 lỗi AI slop.
- **Đề xuất Commit Message:** `docs: initialize project wireframes, cozy woodland tokens and ai review`

---

## Buổi 2: HTML & CSS - Từ Thiết Kế Ra Layout Bento Grid

- **Mục tiêu buổi học:** Dựng layout responsive từ bản thiết kế bằng HTML ngữ nghĩa và CSS thuần; làm quen với CSS Grid (2D) và Flexbox (1D); đảm bảo chuẩn Accessibility (a11y) và đo lường bằng DevTools.
- **Đầu việc thực hành trên OmniStudy:**
  1. Dựng khung giao diện tĩnh của màn **Focus Dashboard** bằng HTML5 ngữ nghĩa (`<aside>` cho `ForestSidebar`, `<header>` cho `ScenicHeaderBanner`, `<main>`, `<section>`, `<article>`).
  2. Sử dụng CSS Grid 12 cột cho bố cục Bento Grid trên Desktop (`Row 1`: `ExamCountdownWidget` 5 cột + `Daily Stats` 7 cột; `Row 2`: `UrgencyTaskList` 8 cột + `Mini Schedule` 4 cột), giữ đồng hồ đếm ngược 4 cột trên 1 hàng ngang; viết Media Query chuyển sang ngăn kéo sidebar và cột dọc trên Mobile.
  3. Áp dụng biến CSS Variables cho bảng tokens **Cozy Woodland Bento** (`#ECE7D8`, `#2F4231`, `#FAF6EC`, `#F1ECDD`, `#4F7043`), họa tiết thiên nhiên SVG tối giản với chuyển động ambient CSS nhẹ nhàng và `@media (prefers-reduced-motion: reduce)`.
  4. Bật chế độ điều hướng phím Tab, thêm `:focus-visible` màu `#4F7043` cho nút bấm và thẻ task; đo điểm bằng Chrome DevTools Lighthouse.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - 1 màn Dashboard tĩnh responsive tốt trên cả Mobile và Desktop.
  - Điểm Lighthouse mục Accessibility đạt $\ge 90$.
  - Deploy trang tĩnh lên GitHub Pages hoặc Netlify.
- **Đề xuất Commit Message:** `feat: build static cozy woodland bento dashboard with semantic html`

---

## Buổi 3: JavaScript & TypeScript Cho React

- **Mục tiêu buổi học:** Thành thạo các hàm mảng và tính bất biến (Immutability); hiểu async/await, Promise và cách gọi API với đủ 3 trạng thái; nắm vững cú pháp TypeScript tối thiểu (`interface`, `type`, `union`).
- **Đầu việc thực hành trên OmniStudy:**
  1. Khai báo các TypeScript Interface định nghĩa cấu trúc dữ liệu miền (`Task`, `UrgencyTier`, `Exam`, `ExamTopic`, `GoogleCalendarEvent`).
  2. Viết các hàm thuần túy (Pure Functions) `calculateUrgencyScore`, tổng hợp tổng giờ công việc (`8.5h`), tính `% Readiness` (`60%` cho `3/5` chủ đề) bằng TypeScript: phân loại chính xác 5 cấp bậc thị giác (`OVERDUE`, `EXTREME`, `HIGH`, `MODERATE`, `LOW`).
  3. Viết script fetch thử nghiệm một Mock API lịch học, xử lý đầy đủ 3 trạng thái `isLoading`, `isError`, `isEmpty` và hiển thị kết quả ra console/DOM.
  4. Mở tab Network trong DevTools phân tích cấu trúc HTTP Request/Response (Status code, Headers, JSON Payload).
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - File logic TypeScript chạy chuẩn, phân loại chính xác các test case deadline và tổng giờ.
  - Code TypeScript không sử dụng kiểu `any`.
- **Đề xuất Commit Message:** `feat: implement task domain types and dynamic urgency calculator`

---

## Buổi 4: React - 1: Tư Duy Component & Render

- **Mục tiêu buổi học:** Khởi tạo project React với Vite + TypeScript; phân rã bản thiết kế thành cây Component; hiểu Props, State, kỹ thuật Lifting State Up; sử dụng Hook `useEffect` đúng mục đích; debug bằng React DevTools.
- **Đầu việc thực hành trên OmniStudy:**
  1. Khởi tạo project React bằng lệnh `npm create vite@latest omnistudy -- --template react-ts`.
  2. Chuyển đổi mã nguồn HTML/CSS từ Buổi 2 thành cây Component React đúng chuẩn `docs/folder-structure.md`: `BentoLayout`, `ForestSidebar`, `ScenicHeaderBanner`, `ExamCountdownWidget`, `UrgencyTaskList`, `UrgencyTaskCard`.
  3. Quản lý danh sách Task bằng `useState`, truyền hàm đánh dấu hoàn thành xuống từng thẻ con và tự động cập nhật lại tổng giờ trong `Daily Stats`.
  4. Viết Custom Hook `useCountdownTimer(targetDate)` cập nhật đồng hồ đếm ngược 4 cột từng giây có hàm dọn dẹp (`clearInterval`) trong `useEffect`.
  5. Dùng React DevTools phát hiện và sửa 1 lỗi re-render thừa giữa đồng hồ đếm ngược và danh sách Task.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - App React chạy mượt mà trên Vite, đồng hồ đếm ngược 4 cột trên 1 hàng ngang nhảy số ổn định.
  - Sơ đồ cây Component chỉ rõ vị trí đặt State và lý do.
- **Đề xuất Commit Message:** `feat: bootstrap vite react app with woodland layout and countdown hook`

---

## Buổi 5: React - 2: Design System & Form UX

- **Mục tiêu buổi học:** Tích hợp Tailwind CSS và thư viện shadcn/ui; xây dựng Form có UX xuất sắc với React Hook Form + Zod (validate từng trường, chống double submit); dựng Shimmer Skeleton và Empty State.
- **Đầu việc thực hành trên OmniStudy:**
  1. Cài đặt Tailwind CSS và cấu hình bảng Tokens **Cozy Woodland Bento** trong `tailwind.config.ts` (`#ECE7D8`, `#2F4231`, `#FAF6EC`, `#F1ECDD`, `#4F7043`, `Fraunces`, `Plus Jakarta Sans`, `JetBrains Mono`). Cài các component shadcn/ui: `Button`, `Input`, `Card`, `Badge`, `Dialog`.
  2. Xây dựng `TaskFormDialog` bằng React Hook Form + Zod: dùng định dạng thời gian `24h` (`DD/MM/YYYY HH:mm`), validate tên task không để trống, hạn chót ở tương lai, giờ ước tính $> 0$, **vô hiệu hóa (`disabled`) nút Submit khi dữ liệu không hợp lệ** và báo lỗi màu `#B94435`.
  3. Xây dựng Form "Thêm Lịch Thi & Tự Động Sinh Task" (`ExamFormDialog` + `ExamPrepDrawer` trượt `360ms`): nhập môn thi, ngày giờ thi `24h` và danh sách chủ đề; tự động chia đều bài ôn chưa xong thành các Task theo ngày.
  4. Dựng **Warm Shimmer Skeleton** (`@keyframes warmShimmer` quét qua `#F1ECDD`/`#FAF6EC`) cho thẻ task khi đang tải và Component Empty State khi mảng rỗng.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - 2 Form hoàn chỉnh dùng định dạng `24h`, validate đầy đủ bằng Zod, disable nút submit khi lỗi và có Toast thông báo.
  - Giao diện có đủ Warm Shimmer Skeleton, Empty State và Error Alert theo chuẩn `DESIGN.md`.
- **Đề xuất Commit Message:** `feat: integrate tailwind shadcn woodland tokens and validated 24h forms`

---

## Buổi 6: React - 3: Dữ Liệu, Routing & Tích Hợp Google Calendar

- **Mục tiêu buổi học:** Xây dựng ứng dụng nhiều trang với React Router; quản lý Server State với TanStack Query v5; xây dựng luồng xác thực đăng nhập và bảo vệ trang (Protected Route).
- **Đầu việc thực hành trên OmniStudy:**
  1. Thiết lập React Router bọc trong `PageTransitionWrapper` (`280ms cubic-bezier(0.22, 1, 0.36, 1)`): `/` (Tổng quan), `/calendar` (Lịch tuần `Thứ 2 - CN`), `/exams/:id` (Chi tiết môn thi), `/login` (Đăng nhập Google), liên kết trực tiếp từ `ForestSidebar`.
  2. Cài đặt TanStack Query v5, tạo Custom Hook `useCalendarEvents` fetch danh sách sự kiện từ Google Calendar API v3 (kèm fallback đọc từ `mockGoogleCalendar.json` nếu lỗi mạng).
  3. Tích hợp nút Đăng nhập Google OAuth 2.0 (thư viện `@react-oauth/google`), lưu Access Token an toàn và xây dựng `ProtectedRoute` chặn truy cập lịch nếu chưa đăng nhập.
  4. Hiển thị sự kiện Google Calendar (tông `--semantic-info` `#3A6F88` / `#E5F1F6`) lên khung Lịch tuần chuẩn `Thứ 2 - CN` xen kẽ với các Task ôn thi.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - Ứng dụng chuyển trang mượt mà không reload trình duyệt với `PageTransitionWrapper`.
  - TanStack Query cache dữ liệu thành công; luồng đăng nhập và xử lý lỗi 401 hoạt động chính xác.
- **Đề xuất Commit Message:** `feat: setup react router transitions tanstack query and google calendar sync`

---

## Buổi 7: Chất Lượng, Làm FE Cùng AI & Đóng Gói Sản Phẩm (Ship)

- **Mục tiêu buổi học:** Đo lường và tối ưu hiệu năng Web (Core Web Vitals); áp dụng Code Splitting (`React.lazy`); áp dụng quy trình kiểm soát AI; đóng gói và Deploy sản phẩm lên Internet.
- **Đầu việc thực hành trên OmniStudy:**
  1. Tách gói mã nguồn trang `/calendar` và modal/drawer nặng bằng `React.lazy` và `Suspense` để giảm kích thước tải ban đầu.
  2. Kiểm chứng `CLS = 0` ở đồng hồ đếm ngược 4 cột 1 hàng ngang (`JetBrains Mono` `tabular-nums`), card hover `-2px`, stagger danh sách `35ms/step` (tối đa `210ms`) và `@media (prefers-reduced-motion: reduce)`.
  3. Dùng AI sinh thêm Component `ExamReadinessBadge` dựa trên `DESIGN.md`, tiến hành review và sửa lỗi AI slop theo checklist (không dùng drop shadow nặng, không dùng viền neon dày).
  4. Đóng gói (`npm run build`) và deploy ứng dụng lên Vercel/Netlify kèm cấu hình biến môi trường `VITE_GOOGLE_CLIENT_ID`.
  5. Quay video demo ngắn 3 phút trình diễn toàn bộ chức năng sản phẩm.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - Đường link website thực tế đang hoạt động trên Vercel/Netlify.
  - Báo cáo điểm số Lighthouse Performance ($\ge 85$) và Accessibility ($\ge 90$).
  - Video demo 3 phút hoàn thiện toàn bộ luồng nghiệp vụ.
- **Đề xuất Commit Message:** `chore: optimize performance with lazy loading and ship production to vercel`
