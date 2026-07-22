---
title: "Triển khai Frontend lên S3 và CloudFront"
date: 2026-07-22
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

# Triển khai Frontend lên S3 và CloudFront

## 1. Cấu hình React

Với Vite, tạo `frontend/.env.production`:

```env
VITE_API_URL=https://YOUR_API_ID.execute-api.ap-southeast-1.amazonaws.com/Prod
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_xxxxxxxxx
VITE_COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxx
VITE_AWS_REGION=ap-southeast-1
```

Không đặt AWS Access Key hoặc Secret Access Key trong Frontend.

## 2. Build

```powershell
cd frontend
npm install
npm run build
```

Vite tạo thư mục `dist`; Create React App thường tạo `build`.

## 3. Tạo S3 Bucket private

1. Mở Amazon S3 và chọn **Create bucket**.
2. Đặt tên duy nhất, ví dụ `smart-attendance-web-ACCOUNT_ID`.
3. Giữ **Block all public access** ở trạng thái bật.
4. Không cần bật S3 static website hosting khi dùng S3 REST origin cùng CloudFront OAC.

Upload:

```powershell
aws s3 sync .\dist s3://TEN_BUCKET --delete
```

## 4. Tạo CloudFront Distribution

- Origin domain: chọn S3 bucket.
- Tạo **Origin Access Control (OAC)**.
- Viewer protocol policy: **Redirect HTTP to HTTPS**.
- Default root object: `index.html`.
- Cập nhật bucket policy theo policy CloudFront đề xuất.

Đối với React Router, tạo Custom Error Response:

| HTTP error | Response page | Response code |
|---|---|---|
| 403 | `/index.html` | 200 |
| 404 | `/index.html` | 200 |

## 5. Cập nhật Frontend

```powershell
npm run build
aws s3 sync .\dist s3://TEN_BUCKET --delete
aws cloudfront create-invalidation `
  --distribution-id DISTRIBUTION_ID `
  --paths "/*"
```

## 6. Kiểm tra

- Website dùng HTTPS.
- S3 bucket không public.
- Đăng nhập Cognito thành công.
- Request API có `Authorization: Bearer ...`.
- Check-in, check-out và lịch sử hoạt động từ Frontend.
