---
title: "Tích hợp Frontend, Cognito và API"
date: 2026-07-22
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---

# Tích hợp Frontend, Cognito và API

> Tên thư mục cũ được giữ nguyên để tránh thay đổi URL. Nội dung đã được cập nhật theo Smart Attendance SaaS.

## 1. Cấu hình Frontend

Tạo hoặc cập nhật file biến môi trường:

```text
frontend/.env
```

Ví dụ với Vite:

```env
VITE_API_URL=https://YOUR_API_ID.execute-api.ap-southeast-1.amazonaws.com
VITE_AWS_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
VITE_COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
```

Không đặt `AWS_SECRET_ACCESS_KEY` trong Frontend.

## 2. Vai trò của AuthContext

File:

```text
frontend/src/context/AuthContext.jsx
```

AuthContext nên quản lý:

- Trạng thái người dùng hiện tại.
- Loading state.
- Đăng nhập.
- Đăng xuất.
- Token hiện tại.
- Làm mới token.
- Kiểm tra nhóm hoặc vai trò người dùng.

## 3. Đính kèm JWT vào API

File:

```text
frontend/src/utils/api.js
```

Mỗi request đã xác thực cần header:

```http
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

Ví dụ:

```javascript
export async function apiRequest(path, options = {}) {
  const token = localStorage.getItem("accessToken");

  const response = await fetch(`${import.meta.env.VITE_API_URL}${path}`, {
    ...options,
    headers: {
      "Content-Type": "application/json",
      ...(token ? { Authorization: `Bearer ${token}` } : {}),
      ...options.headers,
    },
  });

  if (!response.ok) {
    throw new Error(`API error: ${response.status}`);
  }

  return response.json();
}
```

Không sử dụng đoạn mẫu nếu `AuthContext.jsx` của project đã có cơ chế lưu token khác. Hãy dùng cùng một nguồn token trong toàn ứng dụng.

## 4. Kiểm thử Clock-in

```javascript
await apiRequest("/attendance/checkin", {
  method: "POST",
  body: JSON.stringify({
    method: "web",
  }),
});
```

Backend phải lấy danh tính chính từ JWT thay vì tin hoàn toàn vào `userId` do Client gửi.

## 5. Kiểm thử Clock-out và History

```javascript
await apiRequest("/attendance/checkout", {
  method: "POST",
});

const history = await apiRequest("/attendance");
```

Route thực tế phải khớp với `backend/template.yaml`.

## 6. Build Frontend

```powershell
cd C:\Users\nguye\smart-attendance-saas\frontend
npm install
npm run build
```

Thư mục kết quả thường là:

```text
dist
```

## 7. Upload lên Amazon S3

```powershell
aws s3 sync .\dist s3://TEN_BUCKET --delete
```

Giữ bucket ở chế độ private khi phân phối qua CloudFront Origin Access Control.

## 8. Cập nhật CloudFront

Sau khi upload phiên bản mới:

```powershell
aws cloudfront create-invalidation `
  --distribution-id DISTRIBUTION_ID `
  --paths "/*"
```

Đối với React Router, cấu hình CloudFront trả `index.html` cho lỗi 403/404 để tải lại các route như `/dashboard`.
