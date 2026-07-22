---
title: "Kiểm thử, bảo mật và giám sát"
date: 2026-07-22
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

# Kiểm thử, bảo mật và giám sát

## 1. Danh sách kiểm thử chức năng

| Trường hợp | Kết quả mong đợi |
|---|---|
| Đăng nhập đúng | Nhận token và vào Dashboard |
| Đăng nhập sai | Hiển thị lỗi, không lưu token |
| Gọi API không có JWT | 401 hoặc 403 |
| Clock-in lần đầu | Tạo bản ghi |
| Clock-in trùng | Bị từ chối |
| Clock-out chưa Clock-in | Bị từ chối |
| Xem lịch sử | Chỉ thấy dữ liệu đúng người dùng/tenant |
| Cập nhật Profile | Dữ liệu được kiểm tra và cập nhật |
| Export Report | Tạo file hoặc đưa task vào queue |

## 2. Kiểm tra Tenant Isolation

Backend không được chỉ tin `tenantId` từ body request. Tenant cần được lấy từ:

- Cognito custom claim.
- User profile đã được xác minh.
- Bảng mapping người dùng–tenant.
- Authorizer context.

Mọi truy vấn DynamoDB phải chứa tenant key phù hợp.

## 3. Kiểm tra IAM

Mỗi Lambda chỉ nên có quyền cần thiết:

- Clock-in/Clock-out: đọc và ghi Attendance.
- Profile: đọc/ghi dữ liệu Profile.
- Report: đọc dữ liệu, ghi S3 hoặc gửi message.
- Email Worker: nhận SQS và gửi SES.
- Admin: quyền quản trị có giới hạn rõ ràng.

Không dùng policy `Action: "*"` và `Resource: "*"` trong môi trường Production nếu có thể giới hạn.

## 4. Kiểm tra dữ liệu nhạy cảm

- Không commit `.env`.
- Không commit Access Key.
- Dùng Secrets Manager cho API key hoặc credential.
- Dùng KMS cho dữ liệu cần customer-managed key.
- Không ghi token đầy đủ vào log.
- Không trả stack trace cho Client.

## 5. CloudWatch

Theo dõi:

### API Gateway

- Count
- 4XX
- 5XX
- Latency
- IntegrationLatency

### Lambda

- Invocations
- Errors
- Duration
- Throttles
- ConcurrentExecutions

### DynamoDB

- SuccessfulRequestLatency
- ThrottledRequests
- SystemErrors
- ConsumedReadCapacityUnits
- ConsumedWriteCapacityUnits

## 6. Xem log

```powershell
aws logs describe-log-groups `
  --log-group-name-prefix "/aws/lambda/"
```

Tail log bằng SAM:

```powershell
sam logs `
  --stack-name smart-attendance-saas `
  --name TEN_LOGICAL_FUNCTION `
  --tail
```

## 7. Kiểm tra hàng đợi

Nếu sử dụng SQS:

- Kiểm tra `ApproximateNumberOfMessagesVisible`.
- Kiểm tra DLQ.
- Kiểm tra Lambda Event Source Mapping.
- Kiểm tra retry và idempotency.

## 8. Các lớp bảo vệ nâng cao

Kiến trúc tổng thể có thể bổ sung:

- AWS WAF trước CloudFront/API.
- AWS Shield cho bảo vệ DDoS.
- Security Hub để tổng hợp trạng thái bảo mật.
- GuardDuty để phát hiện hành vi bất thường.
- X-Ray để truy vết request.
- CloudTrail để kiểm toán thao tác quản trị.
