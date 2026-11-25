# Hướng Dẫn Đóng Góp & Quy Trình Git (Contributing Guide) - OmniStudy

Tài liệu này quy định quy trình làm việc với Git, quy ước đặt tên nhánh, chuẩn viết Commit Message và kiểm thử trước khi hợp nhất mã nguồn cho dự án **OmniStudy (Smart Exam & Task Planner)**.

---

## 1. Chiến Lược Phân Nhánh (Branching Strategy)

Dự án áp dụng mô hình **GitHub Flow** đơn giản, phù hợp với lộ trình phát triển cá nhân qua 7 buổi học trong [docs/roadmap.md](./docs/roadmap.md):

1. **`main` (Production / Stable Branch):**
   - Luôn ở trạng thái sạch, không lỗi TypeScript, ESLint và đóng gói thành công (`npm run build`).
   - Không commit trực tiếp các thay đổi thử nghiệm lên `main`; nên phát triển trên nhánh tính năng rồi tạo Pull Request hợp nhất vào `main`.
2. **Nhánh tính năng / buổi học (`feature/*`, `fix/*`, `docs/*`):**

| Loại nhánh | Cấu trúc đặt tên | Ví dụ thực tế | Mục đích sử dụng |
| :--- | :--- | :--- | :--- |
| **Feature** | `feature/buoi-<so>-<ten-ngan>` hoặc `feature/<ten-tinh-nang>` | `feature/buoi-3-urgency-calculator` | Hoàn thiện đầu việc thực hành theo từng buổi hoặc thêm tính năng mới |
| **Bugfix** | `fix/<mo-ta-loi>` | `fix/countdown-timer-memory-leak` | Sửa lỗi giao diện, logic tính điểm hoặc trạng thái biên |
| **Docs** | `docs/<noi-dung-tai-lieu>` | `docs/update-buoi-1-wireframes` | Cập nhật tài liệu đặc tả, `DESIGN.md` hoặc `docs/worklog.md` |

---

## 2. Chuẩn Đặt Tên Commit (Conventional Commits)

```text
<type>(<scope>): <subject>
```

### 2.1. Danh mục `<type>` hợp lệ
- `feat`: Thêm giao diện, component, hook hoặc chức năng mới.
- `fix`: Sửa lỗi logic, lỗi giao diện, lỗi TypeScript hoặc lỗi Accessibility.
- `refactor`: Tái cấu trúc component/hàm thuần túy mà không làm thay đổi hành vi.
- `perf`: Tối ưu hóa tốc độ tải trang, code splitting (`React.lazy`), giảm re-render thừa hoặc tối ưu CLS/LCP.
- `style`: Tinh chỉnh CSS/Tailwind tokens (không đổi logic nghiệp vụ).
- `docs`: Cập nhật `README.md`, `DESIGN.md` hoặc các tệp trong `docs/`.
- `chore`: Cập nhật cấu hình Vite, Tailwind, `package.json`, `.gitignore` hoặc deploy.

### 2.2. Danh mục `<scope>` theo các phân hệ của OmniStudy
- `task`: Bảng công việc, form tạo task và động cơ tính điểm khẩn cấp (`UrgencyScore`).
- `exam`: Lịch thi, đồng hồ đếm ngược (`useCountdownTimer`), độ sẵn sàng và bộ tự động sinh task ôn tập.
- `calendar`: Lịch tuần, tích hợp Google OAuth 2.0, Google Calendar API v3 và dữ liệu dự phòng `mockGoogleCalendar.json`.
- `ui`: Design System tokens, Bento Grid layout, Shimmer Skeleton, Empty State, Error Alert và Accessibility.
- `docs`: Tài liệu thiết kế, wireframe, báo cáo AI review và worklog.

### 2.3. Quy tắc viết `<subject>`
- Viết bằng tiếng Anh ngắn gọn, tối đa 72 ký tự, dùng động từ nguyên mẫu hiện tại (`add`, `implement`, `fix`, `update`), không viết hoa chữ cái đầu và không có dấu chấm cuối câu.
- Ví dụ hợp lệ:
  - `feat(task): implement calculateUrgencyScore pure function`
  - `feat(exam): add countdown timer hook with cleanup`
  - `fix(ui): add tabular-nums to prevent countdown layout shift`

---

## 3. Quy Trình Kiểm Chứng Trước Khi Commit

1. Kiểm tra kiểu TypeScript: `npx tsc --noEmit` (từ Buổi 3/4 trở đi).
2. Kiểm tra quy chuẩn mã nguồn: `npm run lint`.
3. Đóng gói thử bản production: `npm run build`.
4. Rà soát bằng `git status` và `git diff` để đảm bảo không đưa tệp `.env`, `.env.local`, `node_modules/` hay `console.log` thừa vào commit.

---

## 4. Nguyên Tắc An Toàn Bảo Mật

- Tuyệt đối không commit `.env` hoặc `.env.local` chứa `VITE_GOOGLE_CLIENT_ID` thật hay bất kỳ thông tin nhạy cảm nào lên Git; chỉ lưu mẫu cấu hình tại `.env.example`.
- Tham khảo thêm quy trình chi tiết tại [docs/git-workflow.md](./docs/git-workflow.md).
