---
title: "Giám sát và bảo mật"
date: 2026-07-22
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---

# Giám sát và bảo mật

## 1. Xem Lambda Logs

```powershell
sam logs `
  --stack-name smart-attendance-workshop `
  --name CheckInFunction `
  --tail
```

Hoặc mở:

```text
CloudWatch → Log groups → /aws/lambda/...
```

## 2. Metric cần theo dõi

### API Gateway

- `Count`
- `4xx`
- `5xx`
- `Latency`
- `IntegrationLatency`

### Lambda

- `Invocations`
- `Errors`
- `Duration`
- `Throttles`
- `ConcurrentExecutions`

### DynamoDB

- `ConsumedReadCapacityUnits`
- `ConsumedWriteCapacityUnits`
- `ThrottledRequests`
- `SuccessfulRequestLatency`
- `SystemErrors`

## 3. Tạo Alarm

```powershell
aws cloudwatch put-metric-alarm `
  --alarm-name "SmartAttendance-CheckIn-Errors" `
  --namespace "AWS/Lambda" `
  --metric-name "Errors" `
  --dimensions Name=FunctionName,Value=TEN_FUNCTION `
  --statistic Sum `
  --period 300 `
  --evaluation-periods 1 `
  --threshold 1 `
  --comparison-operator GreaterThanOrEqualToThreshold `
  --treat-missing-data notBreaching
```

## 4. Checklist bảo mật

- Không nhận `userSub` từ body request.
- Lấy danh tính từ JWT claims.
- Không lưu token, mật khẩu hoặc AWS credential trong Git.
- Không đưa Secret Access Key vào React.
- IAM Role chỉ có quyền cần thiết.
- S3 bucket giữ private và chỉ cho CloudFront OAC truy cập.
- Bật MFA cho tài khoản quản trị.
- Production nên dùng CloudTrail, AWS WAF và cơ chế cảnh báo.
- Không trả stack trace hoặc thông tin nhạy cảm cho Client.

## 5. Kiểm thử lỗi

| Tình huống | Kết quả mong đợi |
|---|---|
| Không có Token | `401 Unauthorized` |
| Token hết hạn | `401 Unauthorized` |
| Check-in hai lần | `409 Conflict` |
| Check-out trước check-in | `404 Not Found` |
| Dữ liệu không hợp lệ | `400 Bad Request` |
| Lỗi DynamoDB | `500` và có log CloudWatch |
