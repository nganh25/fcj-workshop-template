---
title: "Chuẩn bị môi trường"
date: 2026-07-22
weight: 2
chapter: false
pre: "<b>5.2. </b>"
---

# Chuẩn bị môi trường

## 1. Công cụ

Cài đặt:

```text
AWS CLI v2
AWS SAM CLI
Node.js 22 LTS
Git
Visual Studio Code
```

Kiểm tra:

```powershell
aws --version
sam --version
node --version
npm --version
git --version
```

## 2. Cấu hình AWS CLI

```powershell
aws configure
aws sts get-caller-identity
```

Workshop dùng Region:

```powershell
$env:AWS_REGION = "ap-southeast-1"
$env:AWS_DEFAULT_REGION = "ap-southeast-1"
```

## 3. Quyền cần thiết

IAM principal cần quyền tạo CloudFormation, Lambda, API Gateway, Cognito, DynamoDB, IAM Role và CloudWatch Logs. Phần Frontend cần thêm S3 và CloudFront.

Không dùng Root user cho thao tác hằng ngày và nên bật MFA.

## 4. Cấu trúc Backend

Tạo thư mục:

```text
backend
├── template.yaml
└── src
    ├── package.json
    ├── handlers
    │   ├── health.mjs
    │   ├── checkin.mjs
    │   ├── checkout.mjs
    │   └── history.mjs
    └── shared
        ├── database.mjs
        └── response.mjs
```

Kiểm tra template trước khi triển khai:

```powershell
sam validate --lint
```
