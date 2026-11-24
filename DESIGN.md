# Design System & UI Specification - OmniStudy

Tài liệu này định nghĩa hệ thống quy chuẩn thiết kế giao diện (Design System), bảng Tokens chuẩn hóa và các nguyên tắc kỹ thuật bắt buộc nhằm đảm bảo tính nhất quán của ứng dụng **OmniStudy**, đồng thời ngăn chặn triệt để hiện tượng **AI Slop** khi sử dụng các công cụ AI hỗ trợ lập trình giao diện.

---

## 1. Triết Lý Thiết Kế (Design Philosophy)

1. **Dark-First Bento Grid:** Sắp xếp thông tin dạng các khối thẻ Bento có cấu trúc viền mảnh `1px`, mật độ thông tin cao nhưng thông thoáng, kiểm soát độ tương phản chặt chẽ để không gây mỏi mắt cho sinh viên khi học tập vào ban đêm.
2. **Visual Urgency Hierarchy:** Màu sắc rực rỡ chỉ được sử dụng cho thông tin đòi hỏi hành động tức thì (Task quá hạn, Kỳ thi còn dưới 24 giờ). Các khối thông tin còn lại sử dụng tông màu trung tính (Neutral) có kiểm soát.
3. **Tabular Digits First:** Toàn bộ đồng hồ đếm ngược, số giờ còn lại và điểm khẩn cấp bắt buộc dùng font số có độ rộng cố định (`tabular-nums`) để triệt tiêu hiện tượng giật giật layout (Layout Shift) khi cập nhật thời gian thực.
4. **State-Complete Components:** Mọi thành phần hiển thị dữ liệu đều phải được thiết kế và hiện thực hóa đầy đủ 3 trạng thái biên: **Đang tải (Loading Skeleton)**, **Rỗng (Empty State)** và **Lỗi (Error Alert)**.

---

## 2. Bảng Tokens Thiết Kế (Design Tokens)

### 2.1. Hệ Màu Nền & Bề Mặt (Surfaces & Backgrounds)

| CSS Variable | Mã Hex | Mục đích sử dụng |
| :--- | :--- | :--- |
| `--bg-canvas` | `#090A0F` | Nền tổng thể toàn bộ trang web (App Canvas) |
| `--bg-surface-1` | `#11131A` | Nền các khối thẻ Bento Grid, Top Header, Drawer |
| `--bg-surface-2` | `#181B24` | Nền thẻ Task con, ô nhập liệu form, badge phụ |
| `--bg-surface-hover` | `#1F2330` | Trạng thái di chuột (Hover) của dòng dữ liệu và nút phụ |
| `--border-subtle` | `#222634` | Viền mảnh `1px` cấu trúc phân tách giữa các khối |
| `--border-focus` | `#6366F1` | Viền khi Focus vào ô nhập liệu hoặc phần tử được chọn |

### 2.2. Hệ Màu Chữ & Tương Phản (Typography Colors)

| CSS Variable | Mã Hex | Mục đích sử dụng |
| :--- | :--- | :--- |
| `--text-primary` | `#F3F4F6` | Tiêu đề chính, tên môn thi, số giờ đếm ngược (đạt chuẩn WCAG AAA trên nền `#090A0F`) |
| `--text-secondary` | `#9CA3AF` | Mô tả phụ, tên phòng thi, nhãn thời gian |
| `--text-muted` | `#6B7280` | Văn bản gợi ý (Placeholder), task đã gạch bỏ hoàn thành |

### 2.3. Hệ Màu Thương Hiệu & Ngữ Nghĩa (Brand & Semantic Colors)

| Token Name | Mã Hex | Ý nghĩa & Ứng dụng |
| :--- | :--- | :--- |
| `--accent-primary` | `#6366F1` (Indigo 500) | Nút hành động chính (CTA: "Tạo công việc", "Tạo lộ trình") |
| `--accent-hover` | `#4F46E5` (Indigo 600) | Trạng thái Hover của nút chính |
| `--semantic-success` | `#10B981` (Emerald 500) | Đánh dấu hoàn thành task, kỳ thi sẵn sàng 100% |
| `--semantic-warning` | `#F59E0B` (Amber 500) | Lịch thi còn dưới 7 ngày, mức ưu tiên vừa |
| `--semantic-danger` | `#EF4444` (Rose 500) | Lịch thi dưới 24h, công việc quá hạn, nút Hủy |
| `--semantic-info` | `#3B82F6` (Blue 500) | Sự kiện đồng bộ từ Google Calendar API |

### 2.4. Màu Phân Cấp Điểm Khẩn Cấp Động (Dynamic Urgency Tier Colors)

| Phân cấp | Tiêu chí | Màu chữ / Nhãn | Màu nền Badge | Viền trái thẻ (`border-left`) |
| :--- | :--- | :--- | :--- | :--- |
| `OVERDUE` | $h \le 0$ (Quá hạn) | `#FCA5A5` | `rgba(239, 68, 68, 0.20)` | `3px solid #EF4444` (kèm pulse) |
| `EXTREME` | $U \ge 80$ (Cực gấp) | `#FDBA74` | `rgba(249, 115, 22, 0.18)` | `3px solid #F97316` |
| `HIGH` | $60 \le U < 80$ (Ưu tiên cao) | `#FDE047` | `rgba(234, 179, 8, 0.15)` | `3px solid #EAB308` |
| `MODERATE` | $35 \le U < 60$ (Vừa phải) | `#93C5FD` | `rgba(59, 130, 246, 0.15)` | `3px solid #3B82F6` |
| `LOW` | $U < 35$ (Thong thả) | `#9CA3AF` | `rgba(156, 163, 175, 0.12)` | `3px solid #4B5563` |

---

## 3. Hệ Thống Kiểu Chữ (Typography System)

- **Font chữ giao diện chính (`--font-sans`):** `'Inter'`, `-apple-system`, sans-serif.
- **Font chữ số liệu & đếm ngược (`--font-mono`):** `'JetBrains Mono'`, monospace.
- **Quy tắc bắt buộc:** Mọi phần tử hiển thị số đếm (đồng hồ đếm ngược, điểm số $U$, số ngày còn lại) **phải** được khai báo lớp CSS:
  ```css
  font-family: var(--font-mono);
  font-variant-numeric: tabular-nums;
  ```

| Cấp bậc chữ | Kích thước (rem / px) | Độ đậm (Weight) | Chiều cao dòng (Line-height) | Ứng dụng |
| :--- | :--- | :--- | :--- | :--- |
| `Display Timer` | `2.25rem` (`36px`) | `700` (Bold) | `1.1` | Đồng hồ đếm ngược kỳ thi chính |
| `Heading 1` | `1.5rem` (`24px`) | `700` (Bold) | `1.25` | Tiêu đề chính màn Dashboard, Calendar |
| `Heading 2` | `1.125rem` (`18px`) | `600` (SemiBold) | `1.35` | Tiêu đề khối Bento, Tiêu đề Drawer |
| `Heading 3` | `0.9375rem` (`15px`) | `600` (SemiBold) | `1.4` | Tên công việc, Tên môn thi |
| `Body` | `0.875rem` (`14px`) | `400` (Regular) | `1.5` | Nội dung mô tả, văn bản thông thường |
| `Caption / Badge`| `0.75rem` (`12px`) | `500` (Medium) | `1.25` | Nhãn Urgency, thời gian còn lại, phòng thi |

---

## 4. Thang Khoảng Cách & Bo Góc (Spacing & Radius)

1. **Đơn vị cơ sở:** Hệ thống khoảng cách tuân thủ nghiêm ngặt bội số của `4px`:
   - `space-1` (`4px`), `space-2` (`8px`), `space-3` (`12px`), `space-4` (`16px`), `space-6` (`24px`), `space-8` (`32px`).
2. **Độ bo góc (Border Radius):**
   - Khối Bento Card lớn & Modal Dialog: `12px` (`rounded-xl` / `0.75rem`).
   - Thẻ Task con, Ô nhập liệu, Nút bấm: `8px` (`rounded-lg` / `0.5rem`).
   - Nhãn Badge, Tag phân loại: `6px` (`rounded-md` / `0.375rem`).

---

## 5. Đặc Tả 3 Trạng Thái Bắt Buộc Của Thành Phần UI

### 5.1. Trạng Thái Đang Tải (Loading State)
- **Quy tắc:** Tuyệt đối không dùng một vòng tròn xoay (Spinner) che chắn toàn bộ màn hình.
- **Hiện thực:** Sử dụng **Shimmer Skeleton** mô phỏng chính xác hình dáng của thẻ dữ liệu thực tế:
  - Khung xám nền `#181B24`.
  - Hiệu ứng ánh sáng quét ngang nhẹ nhàng (`animate-pulse` hoặc gradient sóng).
  - Chiều cao khung skeleton phải bằng đúng chiều cao trung bình của thẻ thật để tránh giật layout khi nạp xong dữ liệu.

### 5.2. Trạng Thái Rỗng (Empty State)
- **Quy tắc:** Khi danh sách chưa có dữ liệu (chưa có task nào, chưa có môn thi nào, tuần không có sự kiện), không được để màn hình trống trơn.
- **Hiện thực:**
  - Một icon tối giản thể hiện ngữ cảnh (ví dụ: `CalendarX`, `CheckCircle2` từ Lucide Icons, `strokeWidth={1.5}`).
  - Tiêu đề ngắn gọn: *"Chưa có công việc nào trong danh sách"*.
  - Dòng hướng dẫn: *"Bấm nút bên dưới để tạo công việc đầu tiên của bạn"*.
  - Nút bấm hành động chính (Primary Action Button) để người dùng tạo dữ liệu ngay lập tức.

### 5.3. Trạng Thái Lỗi (Error State)
- **Quy tắc:** Khi xảy ra lỗi (lỗi mất mạng, lỗi Google Calendar API 401 hết hạn token), thông báo phải rõ ràng, chỉ rõ nguyên nhân và cách khắc phục.
- **Hiện thực:**
  - Khung viền đỏ mảnh `1px solid #EF4444` trên nền mờ `rgba(239, 68, 68, 0.08)`.
  - Icon cảnh báo `AlertTriangle` màu đỏ.
  - Văn bản nêu rõ lỗi và kèm theo nút bấm `Thử lại` (Retry) hoặc nút `Đăng nhập lại Google`.

---

## 6. Bộ Quy Tắc Phòng Chống "AI Slop" Khi Nhờ AI Viết Code (AI Prompting Rules)

Khi cung cấp yêu cầu cho các công cụ AI (Cursor, Claude Code, v0.dev...), học viên phải yêu cầu AI tuân thủ nghiêm ngặt 5 điều sau:

1. **Cấm lạm dụng Drop Shadow và Glassmorphism mờ mịt:**
   - *Lỗi AI thường mắc:* Thêm bóng đổ đen sì lòe loẹt (`shadow-2xl`) và blur quá độ khiến giao diện mờ mịt, cẩu thả.
   - *Quy tắc:* Thay thế hoàn toàn bằng đường viền cấu trúc sắc nét `1px solid #222634` (`border border-[#222634]`).
2. **Cấm quên thuộc tính `tabular-nums` trên font chữ số liệu:**
   - *Lỗi AI thường mắc:* Dùng font Sans thông thường cho đồng hồ đếm ngược, khiến thẻ giao diện rung lắc khi nhảy giây.
   - *Quy tắc:* Bắt buộc áp dụng `font-mono tabular-nums` cho mọi con số đếm thời gian.
3. **Cấm sinh mã nguồn thiếu các trạng thái biên:**
   - *Lỗi AI thường mắc:* Chỉ viết code cho trường hợp lý tưởng có sẵn dữ liệu, bỏ quên `isLoading`, `isError`, `data.length === 0`.
   - *Quy tắc:* Bắt buộc code đủ 3 nhánh: Component Skeleton khi loading, Component Empty khi mảng rỗng, và Alert khi lỗi.
4. **Cấm tự ý tạo thêm màu gradient tím - hồng generic:**
   - *Lỗi AI thường mắc:* AI có xu hướng áp dụng background gradient tím tím hồng hồng cho mọi thứ.
   - *Quy tắc:* Nền chỉ được dùng màu Canvas tối `#090A0F`, màu nhấn chỉ dùng Indigo 500 `#6366F1`.
5. **Cấm đặt tên class CSS không theo thang tokens:**
   - *Lỗi AI thường mắc:* Dùng các giá trị tùy tiện như `p-[17px]`, `text-[13px]`, `rounded-[15px]`.
   - *Quy tắc:* 100% khoảng cách và bo góc phải tuân thủ Tailwind spacing (`p-2`, `p-4`, `p-6`) và radius (`rounded-lg`, `rounded-xl`).
