# System Architecture Specification - OmniStudy

## 1. Tổng Quan Kiến Trúc Hệ Thống (Client-Side SPA Architecture)

**OmniStudy** được thiết kế theo mô hình **Client-Side Single-Page Application (SPA)** xây dựng trên nền tảng **React 18 + TypeScript 5 + Vite 5**, hoàn toàn không sử dụng Backend riêng hay cơ sở dữ liệu máy chủ (theo phạm vi đã chốt trong [product.md](./product.md)).

Lý do lựa chọn kiến trúc thuần Client-Side:
1. **Phù hợp với mục tiêu đào tạo Frontend 7 buổi:** Tập trung tối đa vào nền tảng HTML/CSS, TypeScript, React component tree, Form UX, quản lý Server State và tối ưu hiệu năng trình duyệt (Core Web Vitals).
2. **Không tốn chi phí hạ tầng máy chủ (Zero-Cost Hosting):** Lưu trữ dữ liệu cá nhân (`tasks`, `exams`) trực tiếp trên trình duyệt qua `localStorage` và giao tiếp trực tiếp với **Google Calendar API v3** thông qua Google OAuth 2.0 phía Client.
3. **Sẵn sàng hoạt động ngoại tuyến / dự phòng:** Khi người dùng chưa đăng nhập Google hoặc gặp lỗi mạng, hệ thống tự động chuyển sang dữ liệu mẫu (`mockGoogleCalendar.json`) để giao diện lịch biểu vẫn vận hành đầy đủ.

```mermaid
flowchart TD
    subgraph Browser["Client Tier (Browser Environment)"]
        Router["React Router (ProtectedRoute & Lazy Pages)"]
        UI["Bento Grid UI (Tailwind CSS + shadcn/ui)"]
        Hooks["Custom Hooks (useCountdownTimer, useUrgency)"]
        PureLib["Pure Domain Logic (calculateUrgencyScore, examTaskGenerator)"]
        TQ["TanStack Query v5 (Server State & Cache)"]
        Storage[("localStorage (Tasks & Exams Persistence)")]
    end

    subgraph External["External APIs (Google Cloud)"]
        GoogleAuth["Google OAuth 2.0 (@react-oauth/google)"]
        GoogleCal["Google Calendar REST API v3 (primary/events)"]
        MockJSON["mockGoogleCalendar.json (Fallback Mode)"]
    end

    Router --> UI
    UI --> Hooks
    Hooks --> PureLib
    UI --> Storage
    UI --> TQ
    GoogleAuth -->|"Access Token (In-Memory / Session)"| TQ
    TQ -->|"GET events (Bearer Token)"| GoogleCal
    TQ -.->|"Fallback khi lỗi mạng / chưa đăng nhập"| MockJSON
```

---

## 2. Luồng Dữ Liệu & Tích Hợp Cốt Lõi (Core Data Flows)

### 2.1. Luồng Xác Thực Google OAuth 2.0 & Đồng Bộ Google Calendar API v3

```mermaid
sequenceDiagram
    autonumber
    actor User as Người dùng
    participant App as OmniStudy React App
    participant Route as ProtectedRoute
    participant OAuth as @react-oauth/google
    participant TQ as TanStack Query v5
    participant GCal as Google Calendar API v3

    User->>App: Truy cập màn Lịch tuần (/calendar)
    App->>Route: Kiểm tra Access Token hợp lệ
    alt Chưa đăng nhập
        Route-->>User: Chuyển hướng tới /login hoặc bật chế độ Fallback
        User->>OAuth: Nhấn "Đăng nhập Google"
        OAuth-->>App: Trả về OAuth2 Access Token (scope: calendar.events.readonly)
    end
    App->>TQ: Kích hoạt useQuery(['calendar-events', weekRange])
    TQ->>GCal: GET /calendar/v3/calendars/primary/events?timeMin=...&timeMax=...
    alt Token hợp lệ (200 OK)
        GCal-->>TQ: Danh sách sự kiện JSON
        TQ-->>App: Render sự kiện Google Calendar (#3B82F6) lên Weekly Grid
    else Token hết hạn (401) hoặc lỗi mạng
        GCal-->>TQ: Trả về lỗi 401 / Network Error
        TQ-->>App: Hiển thị Error Alert kèm nút "Đăng nhập lại Google" hoặc fallback mock JSON
    end
```

### 2.2. Luồng Tính Điểm Khẩn Cấp & Tự Động Sinh Task Ôn Thi

1. **Dynamic Urgency Calculation (`urgency-calculator.ts`):**
   - Toàn bộ các hàm tính $U$, phân loại `UrgencyTier` (`OVERDUE`, `EXTREME`, `HIGH`, `MODERATE`, `LOW`) và sắp xếp danh sách task đều là **Pure Functions** chạy trên TypeScript, dễ viết unit test và không phụ thuộc vào React State.
2. **Smart Exam Task Generator (`exam-task-generator.ts`):**
   - Khi người dùng tạo kỳ thi mới hoặc nhấn *"Tự động tạo lộ trình ôn tập"* trong **Exam Prep Drawer**, hệ thống tính số ngày còn lại $N$ trước `examDate`, duyệt qua $M$ chủ đề chưa hoàn thành và sinh ra $M$ công việc mới có `basePriority = 3`, chia đều hạn chót về trước ngày thi rồi lưu vào `localStorage`.
3. **Cô lập Re-render cho Đồng hồ đếm ngược (`useCountdownTimer`):**
   - Hook đếm ngược từng giây chỉ cập nhật state bên trong `ExamCountdownWidget`, tách biệt hoàn toàn với `UrgencyTaskList` để không gây re-render danh sách công việc mỗi giây.

---

## 3. Chiến Lược Xử Lý 3 Trạng Thái Biên (State-Complete UI)

Mỗi thành phần hiển thị dữ liệu (Dashboard Task Board, Exam Countdown, Weekly Calendar, Exam Prep Drawer) đều bắt buộc xử lý đầy đủ 3 nhánh trạng thái dựa trên cờ của TanStack Query (`isLoading`, `isError`, `data`):
- **Loading:** Render Shimmer Skeleton đúng kích thước thẻ thật (`#181B24`).
- **Empty:** Render thông báo ngữ cảnh kèm nút hành động chính (Primary CTA).
- **Error:** Render khung cảnh báo viền đỏ (`#EF4444`) nêu rõ nguyên nhân và nút `Thử lại` / `Đăng nhập lại Google`.
