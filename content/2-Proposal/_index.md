---
title: "Bản đề xuất"
date: 2026-07-22
weight: 2
chapter: false
pre: "<b>2. </b>"
---

# ĐỀ XUẤT DỰ ÁN SMART ATTENDANCE SAAS

## 1. Tóm tắt điều hành

Smart Attendance SaaS là nền tảng quản lý chấm công đa tổ chức, cho phép doanh nghiệp quản lý người dùng, ghi nhận Clock-in/Clock-out, theo dõi lịch sử, xuất báo cáo và gửi thông báo. Giải pháp được xây dựng trên AWS theo kiến trúc Serverless nhằm giảm công việc vận hành máy chủ, tự động mở rộng và tối ưu chi phí.

## 2. Vấn đề cần giải quyết

Các phương pháp chấm công thủ công hoặc hệ thống đơn lẻ thường gặp các hạn chế:

- Dữ liệu phân tán, khó tổng hợp.
- Quy trình báo cáo mất thời gian.
- Khó quản lý nhiều tổ chức trên cùng một nền tảng.
- Khả năng mở rộng và giám sát còn hạn chế.
- Rủi ro truy cập sai dữ liệu giữa các đơn vị.

## 3. Mục tiêu

- Cung cấp xác thực và phân quyền an toàn.
- Hỗ trợ Clock-in, Clock-out và lịch sử điểm danh.
- Quản lý người dùng và dữ liệu theo Tenant.
- Xuất CSV/Excel và lưu báo cáo trên Amazon S3.
- Gửi email thông báo bằng Amazon SES.
- Hỗ trợ xử lý bất đồng bộ và khả năng mở rộng.
- Triển khai tự động bằng Infrastructure as Code và CI/CD.

## 4. Phạm vi MVP

### Người dùng

- **Admin:** quản lý tổ chức, người dùng và cấu hình.
- **Manager:** theo dõi nhân viên và báo cáo.
- **Employee:** điểm danh, cập nhật hồ sơ và xem lịch sử.

### Chức năng chính

- Đăng ký, đăng nhập và đăng xuất.
- Clock-in và Clock-out.
- Quản lý hồ sơ.
- Lịch sử và tổng hợp điểm danh.
- Xuất báo cáo.
- Email Notification.
- Dashboard và giám sát hệ thống.

## 5. Kiến trúc giải pháp

```text
Route 53
   │
CloudFront + AWS WAF + AWS Shield
   │
Amazon S3 (React SPA)
   │
Amazon Cognito ── JWT
   │
API Gateway HTTP API
   │
AWS Lambda
   ├── Clock-in / Clock-out
   ├── Attendance / Profile / Admin
   └── Report / Subscription
   │
Amazon DynamoDB + AWS KMS
   │
Step Functions + SQS + DLQ
   │
EventBridge + Lambda Worker + Amazon SES
```

## 6. Dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
|---|---|
| Route 53 | DNS và định tuyến tên miền |
| CloudFront | Phân phối Frontend toàn cầu |
| S3 | Lưu React SPA và file báo cáo |
| Cognito | Xác thực và phát hành JWT |
| API Gateway | Cổng truy cập API |
| Lambda | Xử lý nghiệp vụ |
| DynamoDB | Lưu dữ liệu đa Tenant |
| Step Functions | Điều phối workflow |
| SQS và DLQ | Hàng đợi và xử lý lỗi |
| EventBridge | Định tuyến sự kiện |
| SES | Gửi email |
| KMS và Secrets Manager | Mã hóa và quản lý bí mật |
| CloudWatch và X-Ray | Log, metric, alarm và tracing |
| CloudFormation/SAM | Infrastructure as Code |
| CodeBuild/CodePipeline | CI/CD |

## 7. Yêu cầu phi chức năng

- **Bảo mật:** Least Privilege, JWT, mã hóa và Tenant Isolation.
- **Khả năng mở rộng:** dịch vụ Serverless tự động mở rộng.
- **Độ tin cậy:** retry, DLQ và giám sát lỗi.
- **Hiệu năng:** CloudFront cache và truy vấn DynamoDB theo access pattern.
- **Chi phí:** Pay-as-you-go và kiểm soát log/storage lifecycle.
- **Khả năng bảo trì:** IaC, CI/CD và phân tách module rõ ràng.

## 8. Lộ trình

1. Nghiên cứu AWS nền tảng.
2. Phân tích yêu cầu và thiết kế kiến trúc.
3. Xây dựng Cognito, API Gateway, Lambda và DynamoDB.
4. Tích hợp Frontend React.
5. Phát triển báo cáo và xử lý sự kiện.
6. Bảo mật, CI/CD và giám sát.
7. Kiểm thử, tối ưu và demo.

## 9. Rủi ro và giải pháp

| Rủi ro | Giải pháp |
|---|---|
| Sai IAM Policy | Least Privilege và kiểm thử từng Role |
| Lỗi Tenant Isolation | Lấy Tenant từ claim hoặc mapping tin cậy |
| Request xử lý lâu | Chuyển sang Step Functions và SQS |
| Mất message | Retry và Dead-Letter Queue |
| Chi phí tăng | AWS Budgets, lifecycle và theo dõi CloudWatch |
| Lộ credential | Secrets Manager và không commit `.env` |

## 10. Kết quả kỳ vọng

Phiên bản MVP cho phép người dùng đăng nhập, điểm danh, quản lý hồ sơ, xem lịch sử và xuất báo cáo. Hệ thống có thể triển khai lặp lại bằng IaC, theo dõi bằng CloudWatch và mở rộng thêm GPS, QR, PIN, webhook, billing và analytics.
