---
title: "Dọn dẹp tài nguyên"
date: 2026-07-22
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---

# Dọn dẹp tài nguyên

## CloudFront và S3

1. Disable và xóa CloudFront Distribution.
2. Xóa object trong Frontend Bucket và Report Bucket.
3. Xóa cả version nếu S3 Versioning đang bật.

```powershell
aws s3 rm s3://TEN_BUCKET --recursive
```

## Xóa SAM Stack

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend
sam delete --stack-name smart-attendance-saas --region ap-southeast-1
```

## Kiểm tra tài nguyên còn sót

Kiểm tra CloudFormation, Lambda, API Gateway, Cognito, DynamoDB, SQS, Step Functions, EventBridge, SES, S3, CloudFront, CloudWatch, Secrets Manager và KMS.
