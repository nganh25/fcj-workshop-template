---
title: "Phân tích kiến trúc"
date: 2026-07-22
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---

# Phân tích kiến trúc

{{< staticimg src="images/5-Workshop/smart-attendance-architecture.png" alt="Kiến trúc Smart Attendance SaaS" width="100%" >}}

## 1. Luồng xác thực

1. Người dùng đăng nhập Amazon Cognito.
2. Cognito trả về Access Token dạng JWT.
3. Frontend gửi header:

```http
Authorization: Bearer eyJ...
```

4. API Gateway kiểm tra chữ ký, issuer, audience và thời hạn token.
5. Request hợp lệ mới được chuyển đến Lambda.

## 2. Luồng check-in

```text
POST /attendance/checkin
→ JWT Authorizer
→ CheckInFunction
→ DynamoDB PutItem
```

Khóa dữ liệu:

```text
PK = TENANT#demo
SK = ATTENDANCE#{userSub}#{yyyy-mm-dd}
```

`ConditionExpression` ngăn người dùng check-in hai lần trong cùng ngày.

## 3. Luồng check-out

Lambda sử dụng cùng `PK` và `SK`, sau đó cập nhật `checkOutAt`. Nếu chưa có bản ghi check-in, API trả `404`.

## 4. Luồng lịch sử

Lambda Query theo:

```text
PK = TENANT#demo
begins_with(SK, ATTENDANCE#{userSub}#)
```

Danh tính được lấy từ JWT claims, không tin `userId` do Client gửi.

## 5. Nguyên tắc bảo mật

- API nghiệp vụ bắt buộc có JWT.
- IAM Role chỉ có quyền cần thiết trên bảng DynamoDB.
- React không chứa Access Key hoặc Secret Key.
- S3 bucket Frontend giữ private khi dùng CloudFront OAC.
- Log không ghi token hoặc mật khẩu.
