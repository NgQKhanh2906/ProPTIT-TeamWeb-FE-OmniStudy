# Tài Liệu Chuẩn Bị Buổi 1 - OmniStudy (CLB ProPTIT Frontend)

- **Nhánh làm việc:** `feature/chuan-bi-buoi-1`
- **Mục tiêu buổi học:** Chốt đề tài cá nhân xuyên suốt khóa học, vẽ User Flow và Wireframe 2-3 màn chính kèm đủ trạng thái biên (Đang tải, Trống, Lỗi), sinh giao diện bằng AI để phân tích ít nhất 5 lỗi "AI Slop", và lập bảng Design Tokens trực quan.

---

## 1. Chốt Đề Tài & API Ngoài

**OmniStudy - Smart Exam & Task Planner** là ứng dụng web quản lý công việc và lập kế hoạch ôn thi cá nhân theo phong cách **Dark-First Bento Grid**, được thiết kế dành riêng cho sinh viên đại học và người tự học có lịch trình bận rộn. Ứng dụng giải quyết tình trạng học dồn trước kỳ thi (cramming) và trễ hạn deadline bằng động cơ tự động tính **Điểm khẩn cấp động (Dynamic Urgency Score)** kết hợp **Bộ tự động phân bổ bài ôn trước kỳ thi (Exam Prep Kit)**. Hệ thống tích hợp trực tiếp **Google Calendar API v3** (xác thực qua Google OAuth 2.0) để đồng bộ lịch học, lịch họp cá nhân lên lịch tuần và hỗ trợ sắp xếp thời gian biểu trực quan.

- Tài liệu mô tả chi tiết đề tài, công nghệ và lộ trình 7 buổi: [README.md](../README.md)
- Lộ trình chi tiết từng buổi học: [roadmap.md](./roadmap.md)

---

## 2. User Flow & Wireframe 3 Màn Chính (Đủ Trạng Thái Đang Tải, Trống, Lỗi)

### 2.1. Sơ Đồ Luồng Người Dùng (User Flow)

![Sơ đồ User Flow OmniStudy](./assets/buoi-1/user-flow.svg)

### 2.2. Wireframe Màn 1: Focus Dashboard (Bento Grid)

![Wireframe Màn 1 Focus Dashboard đủ 4 trạng thái](./assets/buoi-1/wireframe-1-dashboard.svg)

### 2.3. Wireframe Màn 2: Weekly Calendar & Exam Timeline

![Wireframe Màn 2 Weekly Calendar đủ 4 trạng thái](./assets/buoi-1/wireframe-2-calendar.svg)

### 2.4. Wireframe Màn 3: Exam Prep Drawer

![Wireframe Màn 3 Exam Prep Drawer đủ 4 trạng thái](./assets/buoi-1/wireframe-3-exam-drawer.svg)

### 2.5. Bảng Đối Chiếu 3 Trạng Thái Bắt Buộc Trên 3 Màn

| Màn hình | Đang tải (Loading) | Trống (Empty) | Lỗi (Error) |
| :--- | :--- | :--- | :--- |
| **1. Focus Dashboard** | Shimmer Skeleton `#181B24` đúng chiều cao Exam Countdown, 4 thẻ Task và Mini Schedule | Hiển thị thông báo chưa có task/kỳ thi kèm nút `+ Tạo Công Việc Đầu Tiên` và `+ Thêm Môn Thi` | Khung viền đỏ `#EF4444` tại Mini Schedule (lỗi Google Calendar 401) hoặc banner lỗi kết nối kèm nút `Thử lại` / `Đăng nhập lại Google` |
| **2. Weekly Calendar** | Thanh Indeterminate Progress Bar ở đỉnh kết hợp lưới lịch giảm `opacity: 0.4` | Lưới trống kèm hộp thông báo trung tâm và nút `+ Thêm Lịch Thi / Task` | Thanh Alert đỏ trên đầu bảng lịch báo lỗi tải sự kiện kèm nút `Tải lại trang` |
| **3. Exam Prep Drawer** | 3 dòng skeleton thông tin chung, 1 nút skeleton và 4 dòng checkbox skeleton | Hộp hướng dẫn kèm ô input lớn mời nhập chủ đề đầu tiên; nút sinh task ở trạng thái disabled | Ô ngày thi viền đỏ `#EF4444` và Error Alert ngay dưới nút sinh task khi ngày thi ở quá khứ hoặc không hợp lệ |

- Tài liệu đặc tả đầy đủ User Flow (Mermaid + ASCII) và Wireframe 3 màn: [wireframes.md](./wireframes.md)

---

## 3. Sinh Giao Diện Bằng AI & Phân Tích 5 Lỗi "AI Slop"

Sử dụng công cụ sinh giao diện AI (Google Stitch / v0.dev) với prompt tạo màn **Focus Dashboard** của OmniStudy, sau đó kiểm duyệt và bóc tách 5 lỗi phổ biến:

![Minh họa 5 lỗi AI Slop trên màn Focus Dashboard](./assets/buoi-1/ai-slop-review.svg)

| STT | Lỗi AI Slop | Lý do cần sửa | Giải pháp theo Design System |
| :---: | :--- | :--- | :--- |
| 1 | Thiếu `tabular-nums` trên đồng hồ đếm ngược | Font tỷ lệ có độ rộng chữ số `1` và `8` khác nhau, gây giật layout ngang (CLS) mỗi giây | Dùng `'JetBrains Mono'` kèm `font-variant-numeric: tabular-nums` |
| 2 | Lạm dụng Glassmorphism, Shadow nặng và Gradient tím-hồng | Làm giảm độ sắc nét Bento Grid, giảm độ tương phản chữ và tốn GPU | Dùng nền đặc `#090A0F` / `#11131A` và viền mảnh `1px solid #222634` |
| 3 | Happy Path Fallacy (bỏ quên 3 trạng thái biên) | Ứng dụng bị vỡ hoặc trắng trang khi mới dùng, mạng chậm hoặc hết hạn token API | Bắt buộc thiết kế đủ Shimmer Skeleton, Empty State có CTA, Error Alert có nút Retry |
| 4 | Sụp đổ phân cấp thị giác (Visual Hierarchy Collapse) | Mọi thẻ và nút bấm đều phát sáng ngang nhau khiến mắt không xác định được việc khẩn cấp | Chỉ cấp màu Đỏ/Cam cho task quá hạn hoặc cực gấp; CTA chính dùng `#6366F1` |
| 5 | Độ tương phản chữ thấp dưới chuẩn WCAG | Chữ phụ `#4B5563` trên nền `#11131A` chỉ đạt `~2.8:1` (dưới ngưỡng tối thiểu `4.5:1` của WCAG AA) | Nâng chữ phụ lên `#9CA3AF` (`6.2:1`) và chữ chính `#F3F4F6` (`15.8:1` AAA) |

- Báo cáo phân tích chi tiết Prompt và từng lỗi AI Slop: [ai-review.md](./ai-review.md)

---

## 4. Bảng Design Tokens Nháp (Có Ảnh Hiển Thị Màu Trực Quan)

![Tổng quan hệ thống Design Tokens - OmniStudy](./assets/buoi-1/tokens-overview.svg)

### 4.1. Bảng Màu Chính, Trung Tính & Ngữ Nghĩa

| Nhóm màu | Màu mẫu | Token | Mã Hex | Ứng dụng |
| :--- | :---: | :--- | :--- | :--- |
| Màu chính | ![#6366F1](./assets/buoi-1/swatches/accent-primary.svg) | `--accent-primary` | `#6366F1` | Nút hành động chính (Primary CTA) |
| Màu chính (Hover) | ![#4F46E5](./assets/buoi-1/swatches/accent-hover.svg) | `--accent-hover` | `#4F46E5` | Trạng thái di chuột nút chính |
| Trung tính (Canvas) | ![#090A0F](./assets/buoi-1/swatches/bg-canvas.svg) | `--bg-canvas` | `#090A0F` | Nền tổng thể toàn trang |
| Trung tính (Surface 1) | ![#11131A](./assets/buoi-1/swatches/bg-surface-1.svg) | `--bg-surface-1` | `#11131A` | Nền thẻ Bento Grid, Header, Drawer |
| Trung tính (Surface 2) | ![#181B24](./assets/buoi-1/swatches/bg-surface-2.svg) | `--bg-surface-2` | `#181B24` | Nền thẻ Task con, Input, Shimmer Skeleton |
| Trung tính (Hover) | ![#1F2330](./assets/buoi-1/swatches/bg-surface-hover.svg) | `--bg-surface-hover` | `#1F2330` | Nền dòng/nút phụ khi hover |
| Trung tính (Border) | ![#222634](./assets/buoi-1/swatches/border-subtle.svg) | `--border-subtle` | `#222634` | Viền `1px` phân tách các khối |
| Trung tính (Text 1) | ![#F3F4F6](./assets/buoi-1/swatches/text-primary.svg) | `--text-primary` | `#F3F4F6` | Tiêu đề chính, số đếm (WCAG AAA `15.8:1`) |
| Trung tính (Text 2) | ![#9CA3AF](./assets/buoi-1/swatches/text-secondary.svg) | `--text-secondary` | `#9CA3AF` | Nhãn phụ, phòng thi, mô tả (WCAG AA `6.2:1`) |
| Trung tính (Muted) | ![#6B7280](./assets/buoi-1/swatches/text-muted.svg) | `--text-muted` | `#6B7280` | Placeholder, task đã hoàn thành |
| Thành công | ![#10B981](./assets/buoi-1/swatches/semantic-success.svg) | `--semantic-success` | `#10B981` | Hoàn thành task, tiến độ ôn 100% |
| Cảnh báo | ![#F59E0B](./assets/buoi-1/swatches/semantic-warning.svg) | `--semantic-warning` | `#F59E0B` | Kỳ thi dưới 7 ngày, trạng thái cảnh báo |
| Lỗi / Nguy hiểm | ![#EF4444](./assets/buoi-1/swatches/semantic-danger.svg) | `--semantic-danger` | `#EF4444` | Task quá hạn, kỳ thi dưới 24h, Error Alert |
| Thông tin (API) | ![#3B82F6](./assets/buoi-1/swatches/semantic-info.svg) | `--semantic-info` | `#3B82F6` | Sự kiện đồng bộ từ Google Calendar API |

### 4.2. Font Chữ, Thang Khoảng Cách & Bo Góc

- **Font giao diện (`--font-sans`):** `'Inter'`, `-apple-system`, sans-serif.
- **Font số liệu & đồng hồ (`--font-mono`):** `'JetBrains Mono'`, monospace (bắt buộc kèm `font-variant-numeric: tabular-nums`).
- **Tỷ lệ chữ:** Display Timer `36px` (`700`), Heading 1 `24px` (`700`), Heading 2 `18px` (`600`), Heading 3 `15px` (`600`), Body `14px` (`400`), Caption/Badge `12px` (`500`).
- **Thang khoảng cách (bội số `4px`):** `4px` (`space-1`), `8px` (`space-2`), `12px` (`space-3`), `16px` (`space-4`), `24px` (`space-6`), `32px` (`space-8`).
- **Độ bo góc:** Badge `6px` (`rounded-md`), Input/Button/Task card `8px` (`rounded-lg`), Bento card/Modal `12px` (`rounded-xl`).

- Tài liệu đặc tả đầy đủ Design System & toàn bộ bảng màu (kèm bảng 5 cấp độ Urgency Tiers có ảnh màu mẫu): [DESIGN.md](../DESIGN.md)
