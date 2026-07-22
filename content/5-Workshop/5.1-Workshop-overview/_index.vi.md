---
title: "Tổng quan kiến trúc và mã nguồn"
date: 2026-07-22
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---

# Tổng quan kiến trúc và mã nguồn

## 1. Bài toán

Smart Attendance SaaS là hệ thống quản lý chấm công dành cho nhiều tổ chức. Người dùng có thể đăng nhập, thực hiện Clock-in/Clock-out, xem lịch sử điểm danh, cập nhật hồ sơ và xuất báo cáo.

Hệ thống được thiết kế theo mô hình Serverless nhằm giảm công việc quản trị máy chủ và hỗ trợ tự động mở rộng theo lưu lượng.

{{< staticimg src="images/5-Workshop/smart-attendance-architecture.png" alt="Kiến trúc tổng thể Smart Attendance SaaS" width="100%" >}}

## 2. Luồng xử lý chính

```text
Người dùng
   │
   ▼
Route 53 → CloudFront → React SPA trên S3
   │
   ├── Đăng nhập → Amazon Cognito
   │                    │
   │                    └── JWT Token
   │
   ▼
Amazon API Gateway HTTP API
   │
   ▼
AWS Lambda
   │
   ├── Clock-in
   ├── Clock-out
   ├── Attendance
   ├── Profile
   ├── Report
   └── Admin
   │
   ▼
Amazon DynamoDB
```

Các tác vụ dài như xuất báo cáo và gửi email có thể đi qua AWS Step Functions, Amazon SQS, Amazon EventBridge và Amazon SES.

## 3. Cấu trúc mã nguồn

```text
smart-attendance-saas
├── backend
│   ├── template.yaml
│   ├── buildspec.yml
│   ├── server.js
│   └── src
│       ├── handler
│       │   ├── attendance
│       │   │   └── checkin.js
│       │   ├── profile
│       │   │   └── update.js
│       │   └── reports
│       │       └── export.js
│       ├── shared
│       │   └── database.js
│       └── emailWorker.js
├── frontend
│   ├── package.json
│   └── src
│       ├── context
│       │   └── AuthContext.jsx
│       └── utils
│           └── api.js
└── .github
    └── CODEOWNERS
```

## 4. Vai trò của các file quan trọng

| File | Vai trò |
|---|---|
| `backend/template.yaml` | Khai báo tài nguyên AWS bằng AWS SAM/CloudFormation |
| `backend/server.js` | Chạy Backend mô phỏng ở môi trường local |
| `backend/src/shared/database.js` | Khởi tạo và dùng chung kết nối DynamoDB |
| `attendance/checkin.js` | Xử lý yêu cầu điểm danh |
| `profile/update.js` | Cập nhật hồ sơ người dùng |
| `reports/export.js` | Tổng hợp và xuất báo cáo |
| `frontend/src/context/AuthContext.jsx` | Quản lý trạng thái đăng nhập |
| `frontend/src/utils/api.js` | Gửi request đến Backend và đính kèm JWT |
| `backend/buildspec.yml` | Hướng dẫn AWS CodeBuild thực hiện Build/Test |

## 5. Kết quả mong đợi

Sau workshop:

- Backend được triển khai bằng AWS SAM.
- Frontend kết nối được API.
- Người dùng đăng nhập qua Cognito.
- Clock-in và Clock-out ghi dữ liệu vào DynamoDB.
- Report có thể được tạo hoặc đưa vào hàng đợi xử lý.
- Log được kiểm tra trong CloudWatch.
