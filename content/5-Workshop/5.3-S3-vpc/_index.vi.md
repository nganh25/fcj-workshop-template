---
title: "Chạy thử và triển khai Backend"
date: 2026-07-22
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---

# Chạy thử và triển khai Backend

> Tên thư mục `5.3-S3-vpc` được giữ lại để không làm hỏng cấu trúc URL cũ. Nội dung bên trong đã được chuyển thành phần triển khai Backend Smart Attendance SaaS.

## 1. Kiểm tra Backend local

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend
node server.js
```

Mở:

```text
http://localhost:3000
```

Kiểm tra các endpoint local hiện có trong `server.js`.

## 2. Kiểm tra cấu trúc SAM Template

Trong `backend/template.yaml`, xác nhận các nhóm tài nguyên:

- API Gateway HTTP API.
- Cognito User Pool và App Client.
- Lambda Functions.
- DynamoDB Table.
- SQS Queue và Dead-Letter Queue nếu có.
- Step Functions State Machine nếu có.
- EventBridge Rule và SES Worker nếu có.
- IAM Role/Policy.
- CloudWatch Log Group.
- Output của API URL và tài nguyên quan trọng.

## 3. Validate và Build

```powershell
sam validate --lint
sam build
```

Nếu Build lỗi, kiểm tra:

- `CodeUri`.
- `Handler`.
- Runtime Node.js.
- `package.json`.
- Tên file và tên thư mục.
- Module CommonJS hoặc ESM.
- Quyền truy cập file trên Windows.

## 4. Triển khai lần đầu

```powershell
sam deploy --guided
```

Giá trị gợi ý:

```text
Stack Name: smart-attendance-saas
AWS Region: ap-southeast-1
Confirm changes before deploy: Y
Allow SAM CLI IAM role creation: Y
Disable rollback: N
Save arguments to configuration file: Y
```

## 5. Triển khai các lần tiếp theo

```powershell
sam build
sam deploy
```

## 6. Lấy Output của Stack

```powershell
$STACK = "smart-attendance-saas"

aws cloudformation describe-stacks `
  --stack-name $STACK `
  --query "Stacks[0].Outputs" `
  --output table
```

Lưu lại:

- API URL.
- User Pool ID.
- App Client ID.
- DynamoDB Table Name.
- S3 Report Bucket nếu có.

## 7. Kiểm tra API Gateway

Test route public hoặc health-check:

```powershell
Invoke-RestMethod `
  -Method Get `
  -Uri "API_URL/health"
```

Nếu nhận lỗi:

| Mã lỗi | Nguyên nhân thường gặp |
|---|---|
| 401 | Thiếu hoặc sai JWT |
| 403 | Authorizer/IAM/WAF chặn |
| 404 | Sai route hoặc stage |
| 500 | Lambda lỗi |
| 502 | Lambda trả response không đúng định dạng |

## 8. Xem log Lambda

```powershell
sam logs `
  --stack-name smart-attendance-saas `
  --name TEN_LOGICAL_FUNCTION `
  --tail
```

Hoặc mở CloudWatch Logs trên AWS Console.
