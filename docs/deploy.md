# Deployment Guide - OmniStudy

Tài liệu này hướng dẫn quy trình đóng gói và triển khai ứng dụng **OmniStudy** lên Internet miễn phí theo 2 cột mốc trong [roadmap.md](./roadmap.md): bản giao diện tĩnh ở **Buổi 2** và ứng dụng React hoàn chỉnh ở **Buổi 7**.

---

## 1. Cấu Hình Google Cloud Console (Bắt Buộc Cho Google Calendar Sync)

Trước khi chạy bản production với tính năng đồng bộ Google Calendar API v3, cần cấu hình OAuth 2.0 Client ID trên Google Cloud Console:

1. Tạo Project mới trên Google Cloud Console và bật **Google Calendar API**.
2. Vào **APIs & Services -> OAuth consent screen**:
   - Chọn loại ứng dụng **External**.
   - Khai báo Scope: `https://www.googleapis.com/auth/calendar.events.readonly`.
   - Thêm email cá nhân vào danh sách **Test Users** khi ứng dụng ở trạng thái Testing.
3. Vào **APIs & Services -> Credentials -> Create Credentials -> OAuth client ID (Web application)**:
   - Thêm vào **Authorized JavaScript origins**:
     - Môi trường Local (Vite): `http://localhost:5173`
     - Môi trường Production (Vercel/Netlify): `https://<your-domain>.vercel.app`
4. Sao chép **Client ID** và điền vào biến môi trường `VITE_GOOGLE_CLIENT_ID`.

---

## 2. Triển Khai Bản Tĩnh Buổi 2 (GitHub Pages / Netlify Drop)

Mục tiêu ở Buổi 2 là kiểm thử bản dựng HTML5 + CSS Bento Grid tĩnh trên thiết bị thật và đo điểm Lighthouse Accessibility ($\ge 90$):

1. Đảm bảo trang `index.html` và tệp CSS chạy được bằng đường dẫn tương đối (`./styles.css`).
2. Trên GitHub Repository: vào **Settings -> Pages -> Build and deployment**, chọn nguồn **Deploy from a branch** (`main` hoặc nhánh thực hành Buổi 2).
3. Mở đường dẫn `https://<username>.github.io/<repo>/` và kiểm tra bằng Chrome DevTools Lighthouse trên cả Desktop và Mobile.

---

## 3. Triển Khai Ứng Dụng Hoàn Chỉnh Buổi 7 (Vercel / Netlify)

### 3.1. Chuẩn Bị Trước Khi Đóng Gói (`npm run build`)
Chạy tuần tự các lệnh kiểm tra cục bộ để đảm bảo không có lỗi biên dịch hoặc lỗi kiểu TypeScript:

```bash
npx tsc --noEmit
npm run lint
npm run build
npm run preview
```

Kiểm tra thư mục `dist/` sau khi build:
- Xác nhận trang `/calendar` và modal nặng đã được tách chunk riêng nhờ `React.lazy` và `Suspense`.
- Kiểm tra Lighthouse Performance ($\ge 85$) và Accessibility ($\ge 90$) trên bản `npm run preview`.

### 3.2. Triển Khai Lên Vercel (Khuyến Nghị)
1. Đăng nhập Vercel bằng tài khoản GitHub và chọn **Add New Project -> Import** repository `omnistudy`.
2. Cấu hình Build Settings:
   - **Framework Preset:** `Vite`
   - **Build Command:** `npm run build`
   - **Output Directory:** `dist`
3. Tại mục **Environment Variables**, thêm biến:
   - `VITE_GOOGLE_CLIENT_ID` = `<Client ID từ Google Cloud Console>`
4. Nhấn **Deploy**. Nếu sử dụng React Router (`BrowserRouter`), thêm tệp `vercel.json` ở thư mục gốc để tránh lỗi `404` khi tải lại các đường dẫn như `/calendar` hoặc `/exams/1`:

```json
{
  "rewrites": [
    { "source": "/(.*)", "destination": "/index.html" }
  ]
}
```

### 3.3. Triển Khai Dự Phòng Lên Netlify
- **Build Command:** `npm run build`
- **Publish Directory:** `dist`
- Thêm tệp `public/_redirects` với nội dung `/* /index.html 200` để hỗ trợ định tuyến phía Client (SPA fallback).
