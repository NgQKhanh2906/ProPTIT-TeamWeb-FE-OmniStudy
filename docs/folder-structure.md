# Project Folder Structure - OmniStudy

Dự án **OmniStudy** được tổ chức theo cấu trúc **Single-Repository Frontend (Vite + React 18 + TypeScript)**, tách biệt rõ ràng giữa khu vực tài liệu đặc tả (`docs/`) và mã nguồn ứng dụng (`src/` sẽ được khởi tạo chính thức từ Buổi 4 theo [roadmap.md](./roadmap.md)).

---

## 1. Sơ Đồ Cây Thư Mục Mục Tiêu (Target File Tree)

```text
omnistudy/
├── .env.example                    # Mẫu biến môi trường (VITE_GOOGLE_CLIENT_ID)
├── .gitignore                      # Loại trừ node_modules, dist, .env.local
├── AGENTS.md                       # Quy chuẩn kiến trúc & hướng dẫn cho AI Agent
├── CONTRIBUTING.md                 # Quy ước nhánh Git và Conventional Commits
├── DESIGN.md                       # Hệ thống Design Tokens & quy tắc chống AI Slop
├── LICENSE                         # Giấy phép MIT
├── README.md                       # Tổng quan đề tài, tính năng và Quick Start
├── package.json                    # Dependencies và scripts (từ Buổi 4)
├── tailwind.config.ts              # Cấu hình Tailwind CSS Dark Bento (từ Buổi 5)
├── tsconfig.json                   # Cấu hình TypeScript Strict Mode (từ Buổi 3/4)
├── docs/
│   ├── README.md                   # Mục lục toàn bộ tài liệu kỹ thuật
│   ├── product.md                  # Tầm nhìn sản phẩm, Persona, Scope
│   ├── spec.md                     # Đặc tả chức năng FR/NFR và công thức Urgency Score
│   ├── roadmap.md                  # Lộ trình triển khai 7 buổi học
│   ├── wireframes.md               # User Flow và Wireframe 3 màn hình chính
│   ├── ai-review.md                # Phân tích 5 lỗi AI Slop và cách khắc phục
│   ├── architecture.md             # Kiến trúc Client-side SPA & luồng dữ liệu
│   ├── folder-structure.md         # Tài liệu cấu trúc thư mục dự án
│   ├── deploy.md                   # Hướng dẫn deploy GitHub Pages & Vercel/Netlify
│   ├── git-workflow.md             # Quy trình Git chi tiết theo 7 buổi
│   ├── worklog.md                  # Nhật ký tiến độ thực hiện 7 buổi
│   ├── tai_lieu_chuan_bi/
│   │   └── buoi-1-chuan-bi.md      # Tài liệu tổng hợp nghiệm thu Buổi 1
│   └── assets/
│       └── buoi-1/                 # Hình SVG User Flow, Wireframe, AI Review và Swatches
└── src/                            # Mã nguồn React + TypeScript (khởi tạo ở Buổi 4)
    ├── components/
    │   ├── common/                 # Button, Input, Skeleton, Dialog, EmptyState, ErrorAlert
    │   ├── exam/                   # ExamCountdownWidget, ExamPrepDrawer, ExamCard, ExamReadinessBadge
    │   ├── layout/                 # TopHeader, BentoLayout, ProtectedRoute
    │   └── task/                   # UrgencyTaskList, UrgencyTaskCard, TaskFormDialog
    ├── hooks/                      # useCountdownTimer.ts, useUrgency.ts, useCalendarEvents.ts
    ├── lib/                        # urgency-calculator.ts, exam-task-generator.ts, date-utils.ts
    ├── pages/                      # DashboardPage.tsx, WeeklyCalendarPage.tsx, LoginPage.tsx
    ├── services/                   # google-calendar-service.ts, mockGoogleCalendar.json
    ├── types/                      # task.ts, exam.ts, calendar.ts
    ├── App.tsx                     # Cấu hình React Router và Code Splitting (React.lazy)
    ├── main.tsx                    # Điểm khởi chạy ứng dụng, bọc QueryClientProvider & GoogleOAuthProvider
    └── index.css                   # Khai báo Tailwind CSS và CSS Variables từ DESIGN.md
```

---

## 2. Trách Nhiệm Từng Khu Vực Mã Nguồn (`src/`)

- **`src/types/`**: Định nghĩa toàn bộ TypeScript interfaces (`Task`, `UrgencyTier`, `Exam`, `ExamTopic`, `GoogleCalendarEvent`) theo chuẩn Strict TypeScript, không dùng `any`.
- **`src/lib/`**: Chứa các hàm thuần túy (Pure Functions) phục vụ tính toán nghiệp vụ:
  - `urgency-calculator.ts`: Cài đặt công thức tính $U \in [0, 100]$ và phân cấp 5 mức độ (`OVERDUE`, `EXTREME`, `HIGH`, `MODERATE`, `LOW`).
  - `exam-task-generator.ts`: Phân bổ danh sách chủ đề môn thi thành các Task ôn tập trải đều trước ngày thi.
  - `date-utils.ts`: Tiện ích xử lý khoảng thời gian tuần, tính số giờ còn lại bằng `date-fns`.
- **`src/services/`**: Chứa `google-calendar-service.ts` gọi Google Calendar API v3 (`GET /calendars/primary/events`) và `mockGoogleCalendar.json` dùng cho chế độ dự phòng (Fallback Mode).
- **`src/hooks/`**: Đóng gói logic state tái sử dụng, tiêu biểu là `useCountdownTimer(targetDate)` có cleanup `clearInterval` chống rò rỉ bộ nhớ.
- **`src/components/`**: Chia theo 4 nhóm rõ ràng (`common`, `layout`, `task`, `exam`), mỗi component hiển thị dữ liệu đều xử lý đủ 3 trạng thái Loading / Empty / Error theo `DESIGN.md`.
