# Execution Roadmap - OmniStudy (Frontend 2026)

Lộ trình phát triển dự án **OmniStudy** gồm **7 chặng thực hành tuần tự** tương ứng với 7 buổi học của chương trình đào tạo Frontend. Mỗi chặng được thiết kế theo mô hình: **Học lý thuyết trên lớp -> Thực hành tính năng cụ thể vào OmniStudy -> Tự kiểm tra và nộp bài.**

---

## Buổi 1: Chốt Đề Tài, Tư Duy UI/UX & Bộ Quy Chuẩn Thiết Kế

- **Mục tiêu buổi học:** Hiểu cách thức hoạt động của Web; nắm vững UX Heuristics (NN/g), Visual Hierarchy, Spacing thang 4/8px; biết nhận diện lỗi AI Slop; chốt đề tài và lập bảng tokens.
- **Đầu việc thực hành trên OmniStudy:**
  1. Khởi tạo repository GitHub `omnistudy`, tạo file `README.md` với mô tả 3-5 câu về bài toán giải quyết cho sinh viên ôn thi.
  2. Vẽ Wireframe 2-3 màn chính (Focus Dashboard, Weekly Calendar, Exam Prep Drawer) trên Excalidraw/Figma, vẽ đủ 3 trạng thái: Đang tải (Loading Skeleton), Rỗng (Empty), Lỗi (Error).
  3. Dùng Google Stitch / v0.dev sinh giao diện màn Dashboard, phân tích chỉ ra ít nhất 5 lỗi AI Slop kèm lý do trong `docs/ai-review.md`.
  4. Lập bảng Tokens nháp và bộ quy tắc AI trong `DESIGN.md`: Màu Canvas `#090A0F`, Thẻ `#11131A`, Viền `#222634`, Accent `#6366F1`, Font `Inter` & `JetBrains Mono` (`tabular-nums`).
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - Repo GitHub có file `README.md` mô tả đúng chuẩn.
  - File `DESIGN.md` lưu đầy đủ bảng tokens và quy tắc chống AI slop.
  - File `docs/wireframes.md` chứa luồng và bản vẽ 3 màn hình.
  - File `docs/ai-review.md` chứa phân tích 5 lỗi AI slop.
- **Đề xuất Commit Message:** `docs: initialize project wireframes, tokens and ai review`

---

## Buổi 2: HTML & CSS - Từ Thiết Kế Ra Layout Bento Grid

- **Mục tiêu buổi học:** Dựng layout responsive từ bản thiết kế bằng HTML ngữ nghĩa và CSS thuần; làm quen với CSS Grid (2D) và Flexbox (1D); đảm bảo chuẩn Accessibility (a11y) và đo lường bằng DevTools.
- **Đầu việc thực hành trên OmniStudy:**
  1. Dựng khung giao diện tĩnh của màn **Focus Dashboard** bằng HTML5 ngữ nghĩa (`<header>`, `<main>`, `<section>`, `<article>`, `<aside>`).
  2. Sử dụng CSS Grid 12 cột cho bố cục Bento Grid trên Desktop, viết Media Query chuyển sang Flexbox cột dọc cho Mobile.
  3. Áp dụng biến CSS Variables cho bảng tokens màu sắc, khoảng cách và bo góc.
  4. Bật chế độ điều hướng phím Tab, thêm `:focus-visible` cho nút bấm và thẻ task; đo điểm bằng Chrome DevTools Lighthouse.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - 1 màn Dashboard tĩnh responsive tốt trên cả Mobile và Desktop.
  - Điểm Lighthouse mục Accessibility đạt $\ge 90$.
  - Deploy trang tĩnh lên GitHub Pages hoặc Netlify.
- **Đề xuất Commit Message:** `feat: build static bento dashboard with semantic html and responsive grid`

---

## Buổi 3: JavaScript & TypeScript Cho React

- **Mục tiêu buổi học:** Thành thạo các hàm mảng và tính bất biến (Immutability); hiểu async/await, Promise và cách gọi API với đủ 3 trạng thái; nắm vững cú pháp TypeScript tối thiểu (`interface`, `type`, `union`).
- **Đầu việc thực hành trên OmniStudy:**
  1. Khai báo các TypeScript Interface định nghĩa cấu trúc dữ liệu miền (`Task`, `UrgencyTier`, `Exam`, `GoogleCalendarEvent`).
  2. Viết hàm thuần túy (Pure Function) `calculateUrgencyScore` bằng TypeScript: Tính toán điểm khẩn cấp dựa trên hạn chót và giờ ước tính, phân loại thành 5 cấp bậc thị giác (`OVERDUE`, `EXTREME`, `HIGH`, `MODERATE`, `LOW`).
  3. Viết script fetch thử nghiệm một Mock API lịch học, xử lý đầy đủ 3 trạng thái `isLoading`, `isError`, `isEmpty` và hiển thị kết quả ra console/DOM.
  4. Mở tab Network trong DevTools phân tích cấu trúc HTTP Request/Response (Status code, Headers, JSON Payload).
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - File logic TypeScript chạy chuẩn, phân loại chính xác các test case deadline.
  - Code TypeScript không sử dụng kiểu `any`.
- **Đề xuất Commit Message:** `feat: implement task domain types and dynamic urgency calculator`

---

## Buổi 4: React - 1: Tư Duy Component & Render

- **Mục tiêu buổi học:** Khởi tạo project React với Vite + TypeScript; phân rã bản thiết kế thành cây Component; hiểu Props, State, kỹ thuật Lifting State Up; sử dụng Hook `useEffect` đúng mục đích; debug bằng React DevTools.
- **Đầu việc thực hành trên OmniStudy:**
  1. Khởi tạo project React bằng lệnh `npm create vite@latest omnistudy -- --template react-ts`.
  2. Chuyển đổi mã nguồn HTML/CSS từ Buổi 2 thành cây Component React: `DashboardLayout`, `ExamCountdownWidget`, `UrgencyTaskList`, `UrgencyTaskCard`.
  3. Quản lý danh sách Task bằng `useState`, truyền hàm đánh dấu hoàn thành xuống từng thẻ con.
  4. Viết Custom Hook `useCountdownTimer(targetDate)` cập nhật đồng hồ đếm ngược từng giây có hàm dọn dẹp (cleanup function) trong `useEffect`.
  5. Dùng React DevTools phát hiện và sửa 1 lỗi re-render thừa khi nhập liệu tìm kiếm.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - App React chạy mượt mà trên Vite, đồng hồ đếm ngược nhảy số ổn định.
  - Sơ đồ cây Component chỉ rõ vị trí đặt State và lý do.
- **Đề xuất Commit Message:** `feat: bootstrap vite react app with countdown hook and task component tree`

---

## Buổi 5: React - 2: Design System & Form UX

- **Mục tiêu buổi học:** Tích hợp Tailwind CSS và thư viện shadcn/ui; xây dựng Form có UX xuất sắc với React Hook Form + Zod (validate từng trường, chống double submit); dựng Skeleton và Empty State.
- **Đầu việc thực hành trên OmniStudy:**
  1. Cài đặt Tailwind CSS và cấu hình bảng Tokens Dark Bento trong `tailwind.config.ts`. Cài các component shadcn/ui: `Button`, `Input`, `Card`, `Badge`, `Dialog`.
  2. Xây dựng Form "Tạo Task Mới" bằng React Hook Form + Zod: Validate tên task không để trống, hạn chót ở tương lai, giờ ước tính $> 0$.
  3. Xây dựng Form "Thêm Lịch Thi & Tự Động Sinh Task": Nhập môn thi, ngày thi và danh sách chủ đề; tự động chia đều bài ôn thành các Task theo ngày.
  4. Dựng Shimmer Skeleton cho thẻ task khi đang tải dữ liệu và Component Empty State khi mảng rỗng.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - 2 Form hoàn chỉnh có validate đầy đủ bằng Zod và thông báo lỗi trực quan.
  - Giao diện có đủ Shimmer Skeleton và Empty State theo chuẩn `DESIGN.md`.
- **Đề xuất Commit Message:** `feat: integrate tailwind shadcn and build validated forms with zod`

---

## Buổi 6: React - 3: Dữ Liệu, Routing & Tích Hợp Google Calendar

- **Mục tiêu buổi học:** Xây dựng ứng dụng nhiều trang với React Router; quản lý Server State với TanStack Query v5; xây dựng luồng xác thực đăng nhập và bảo vệ trang (Protected Route).
- **Đầu việc thực hành trên OmniStudy:**
  1. Thiết lập React Router: `/` (Dashboard), `/calendar` (Lịch tuần), `/exams/:id` (Chi tiết môn thi), `/login` (Đăng nhập Google).
  2. Cài đặt TanStack Query v5, tạo Custom Hook fetch danh sách sự kiện từ Google Calendar API (kèm fallback đọc từ file mock JSON nếu lỗi mạng).
  3. Tích hợp nút Đăng nhập Google OAuth 2.0 (thư viện `@react-oauth/google`), lưu Access Token an toàn và xây dựng `ProtectedRoute` chặn truy cập lịch nếu chưa đăng nhập.
  4. Hiển thị sự kiện Google Calendar lên khung lịch tuần xen kẽ với các Task ôn thi.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - Ứng dụng chuyển trang mượt mà không reload trình duyệt.
  - TanStack Query cache dữ liệu thành công; luồng đăng nhập và xử lý lỗi 401 hoạt động chính xác.
- **Đề xuất Commit Message:** `feat: setup react router tanstack query and google calendar integration`

---

## Buổi 7: Chất Lượng, Làm FE Cùng AI & Đóng Gói Sản Phẩm (Ship)

- **Mục tiêu buổi học:** Đo lường và tối ưu hiệu năng Web (Core Web Vitals); áp dụng Code Splitting (`React.lazy`); áp dụng quy trình kiểm soát AI; đóng gói và Deploy sản phẩm lên Internet.
- **Đầu việc thực hành trên OmniStudy:**
  1. Tách gói mã nguồn trang `/calendar` và modal chi tiết bằng `React.lazy` và `Suspense` để giảm kích thước tải ban đầu.
  2. Khắc phục triệt để lỗi giật layout CLS ở đồng hồ đếm ngược bằng thuộc tính `tabular-nums` trên font `JetBrains Mono`.
  3. Dùng AI sinh thêm Component `ExamReadinessBadge` dựa trên `DESIGN.md`, tiến hành review và sửa lỗi AI slop theo checklist.
  4. Đóng gói (`npm run build`) và deploy ứng dụng lên Vercel/Netlify kèm cấu hình biến môi trường `VITE_GOOGLE_CLIENT_ID`.
  5. Quay video demo ngắn 3 phút trình diễn toàn bộ chức năng sản phẩm.
- **Tiêu chuẩn nghiệm thu (Deliverables):**
  - Đường link website thực tế đang hoạt động trên Vercel/Netlify.
  - Báo cáo điểm số Lighthouse Performance đạt yêu cầu.
  - Video demo 3 phút hoàn thiện toàn bộ luồng nghiệp vụ.
- **Đề xuất Commit Message:** `chore: optimize performance with lazy loading and ship production to vercel`
