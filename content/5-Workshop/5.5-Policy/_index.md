---
title: "Kiểm thử, bảo mật và giám sát"
date: 2026-07-22
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

# Kiểm thử, bảo mật và giám sát

## Kiểm thử

| Trường hợp | Kết quả |
|---|---|
| Không có JWT | 401/403 |
| Clock-in lần đầu | Tạo bản ghi |
| Clock-in trùng | 409 hoặc lỗi nghiệp vụ |
| Clock-out trước Clock-in | Bị từ chối |
| Xem lịch sử | Chỉ thấy dữ liệu đúng người dùng/Tenant |

## Bảo mật

- Lấy `userSub` và Tenant từ claim hoặc mapping tin cậy.
- Không commit `.env` hoặc credential.
- Dùng IAM Least Privilege.
- Dùng Secrets Manager và KMS khi cần.
- Giữ S3 Frontend private sau CloudFront OAC.

## Giám sát

Theo dõi API Gateway `4XX/5XX/Latency`, Lambda `Errors/Duration/Throttles`, DynamoDB `ThrottledRequests/Latency` và SQS DLQ.

```powershell
sam logs --stack-name smart-attendance-saas --name TEN_FUNCTION --tail
```
