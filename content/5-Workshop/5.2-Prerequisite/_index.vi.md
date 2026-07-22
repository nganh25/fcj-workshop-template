---
title: "Chuẩn bị môi trường"
date: 2026-07-22
weight: 2
chapter: false
pre: "<b>5.2. </b>"
---

# Chuẩn bị môi trường

## 1. Công cụ cần cài đặt

Kiểm tra:

```powershell
node --version
npm --version
git --version
aws --version
sam --version
```

Khuyến nghị:

```text
Node.js 20 hoặc 22
npm
Git
AWS CLI v2
AWS SAM CLI
Visual Studio Code
```

## 2. Kiểm tra AWS CLI

```powershell
aws configure
aws sts get-caller-identity
```

Region dùng trong workshop:

```text
ap-southeast-1
```

Thiết lập biến môi trường:

```powershell
$env:AWS_REGION = "ap-southeast-1"
$env:AWS_DEFAULT_REGION = "ap-southeast-1"
```

## 3. Mở project

```powershell
cd C:\Users\nguye\smart-attendance-saas
code .
```

Kiểm tra trạng thái Git:

```powershell
git status
git branch
```

## 4. Cài thư viện Backend

```powershell
cd backend
npm install
```

Các cảnh báo package deprecated không nhất thiết làm project ngừng chạy. Tuy nhiên, không nên chạy `npm audit fix --force` ngay trên nhánh chính vì có thể tạo breaking change.

Kiểm tra chi tiết:

```powershell
npm audit
```

## 5. Chạy Backend local

```powershell
node server.js
```

Kết quả mong đợi:

```text
Server đang chạy tại http://localhost:3000
```

Giữ cửa sổ Terminal này đang mở.

## 6. Cài và chạy Frontend

Mở Terminal mới:

```powershell
cd C:\Users\nguye\smart-attendance-saas\frontend
npm install
npm run dev
```

Nếu project dùng Create React App:

```powershell
npm start
```

## 7. Kiểm tra AWS SAM Template

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend
sam validate --lint
```

Nếu template hợp lệ, tiếp tục:

```powershell
sam build
```

## 8. Kiểm tra trước khi triển khai

- AWS CLI đăng nhập đúng Account.
- Region đúng.
- Không có Access Key trong source code.
- `template.yaml` không còn giá trị placeholder.
- Frontend có API URL phù hợp cho từng môi trường.
- Các file nhạy cảm nằm trong `.gitignore`.
