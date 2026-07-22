---
title: "Dọn dẹp tài nguyên"
date: 2026-07-22
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---

# Dọn dẹp tài nguyên

Sau khi thực hành, xóa tài nguyên không còn sử dụng để hạn chế chi phí.

## 1. Sao lưu dữ liệu cần thiết

Trước khi xóa:

- Tải các report cần giữ.
- Xuất dữ liệu DynamoDB nếu cần.
- Lưu CloudWatch Logs phục vụ báo cáo.
- Kiểm tra file cấu hình và Output của Stack.

## 2. Xóa dữ liệu S3

```powershell
aws s3 rm s3://TEN_FRONTEND_BUCKET --recursive
aws s3 rm s3://TEN_REPORT_BUCKET --recursive
```

Nếu Versioning đang bật, cần xóa cả version và delete marker.

## 3. Xóa hoặc Disable CloudFront

1. Mở CloudFront Console.
2. Chọn Distribution.
3. Chọn Disable.
4. Chờ trạng thái deploy hoàn tất.
5. Chọn Delete.

## 4. Xóa SAM Stack

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend

sam delete `
  --stack-name smart-attendance-saas `
  --region ap-southeast-1
```

## 5. Kiểm tra tài nguyên còn sót

Kiểm tra:

- CloudFormation.
- Lambda.
- API Gateway.
- Cognito.
- DynamoDB.
- SQS và DLQ.
- Step Functions.
- EventBridge.
- SES.
- S3.
- CloudFront.
- CloudWatch Log Groups và Alarms.
- Secrets Manager.
- KMS key do workshop tạo.

## 6. Kết quả workshop

Bạn đã thực hiện:

- Đọc cấu trúc mã nguồn Smart Attendance SaaS.
- Chạy Frontend và Backend local.
- Validate và Build AWS SAM Template.
- Triển khai Backend Serverless.
- Tích hợp Cognito JWT với Frontend.
- Kiểm thử Clock-in, Clock-out, Attendance, Profile và Report.
- Triển khai Frontend qua S3 và CloudFront.
- Rà soát IAM, Tenant Isolation và dữ liệu nhạy cảm.
- Theo dõi hệ thống bằng CloudWatch.
- Dọn dẹp tài nguyên.

## 7. Hướng phát triển

- Hoàn thiện Multi-tenant SaaS.
- Thêm QR, PIN và GPS.
- Bổ sung Dashboard thống kê.
- Tự động xuất CSV/Excel.
- Dùng Step Functions và SQS cho tác vụ dài.
- Dùng EventBridge và SES cho thông báo.
- Tự động triển khai bằng CodePipeline và CodeBuild.
- Bổ sung AWS WAF, Security Hub, GuardDuty và X-Ray.
