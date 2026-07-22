---
title: "Dọn dẹp tài nguyên"
date: 2026-07-22
weight: 9
chapter: false
pre: "<b>5.9. </b>"
---

# Dọn dẹp tài nguyên

Sau khi thực hành, xóa tài nguyên không còn sử dụng để tránh phát sinh chi phí.

## 1. Xóa CloudFront

1. Chọn Distribution.
2. Chọn **Disable**.
3. Chờ cập nhật hoàn tất.
4. Chọn **Delete**.

## 2. Xóa dữ liệu và S3 Bucket

```powershell
aws s3 rm s3://TEN_BUCKET --recursive
aws s3api delete-bucket `
  --bucket TEN_BUCKET `
  --region ap-southeast-1
```

Nếu bật Versioning, cần xóa cả object versions và delete markers.

## 3. Xóa SAM Stack

```powershell
sam delete `
  --stack-name smart-attendance-workshop `
  --region ap-southeast-1
```

Stack sẽ xóa API Gateway, Lambda, DynamoDB, Cognito, IAM Role và Log Group đã khai báo.

## 4. Kiểm tra tài nguyên còn sót

Kiểm tra CloudFormation, Lambda, API Gateway, DynamoDB, Cognito, CloudWatch, S3, CloudFront và CloudWatch Alarms.

## 5. Kết quả workshop

Bạn đã hoàn thành:

- Cognito User Pool và JWT Authorizer.
- HTTP API cho check-in/check-out.
- Lambda Node.js và DynamoDB.
- Triển khai Infrastructure as Code bằng AWS SAM.
- Kiểm thử API bằng PowerShell.
- React SPA trên S3 và CloudFront.
- CloudWatch Logs, Metrics và Alarm.
- Quy trình dọn dẹp tài nguyên.

## Hướng phát triển

- Multi-tenant và phân quyền Manager/Admin/Employee.
- GPS, QR hoặc PIN cho phiên điểm danh.
- Xuất CSV/Excel vào S3.
- SQS và Step Functions cho báo cáo bất đồng bộ.
- EventBridge và SES cho Email Notification.
- AWS WAF, Shield, KMS và Secrets Manager.
- CodePipeline/CodeBuild cho CI/CD.
- AWS X-Ray hoặc OpenTelemetry cho tracing.
