---
title: "Workshop"
date: 2026-07-22
weight: 5
chapter: false
pre: "<b>5. </b>"
---

# Xây dựng Smart Attendance SaaS Serverless trên AWS

## Tổng quan

Workshop này hướng dẫn xây dựng phiên bản MVP của **Smart Attendance SaaS** theo kiến trúc Serverless. Người dùng đăng nhập bằng Amazon Cognito, gửi yêu cầu điểm danh qua Amazon API Gateway và AWS Lambda, còn dữ liệu được lưu trong Amazon DynamoDB. Backend được triển khai bằng AWS SAM; React SPA được phân phối bằng Amazon S3 và Amazon CloudFront.

## Mục tiêu

- Tạo Amazon Cognito User Pool và JWT Authorizer.
- Xây dựng HTTP API gồm check-in, check-out và lịch sử điểm danh.
- Lưu dữ liệu trong DynamoDB.
- Triển khai Backend bằng AWS SAM.
- Kiểm thử API bằng PowerShell.
- Triển khai React SPA trên S3 và CloudFront.
- Theo dõi hệ thống bằng CloudWatch.
- Dọn dẹp tài nguyên để tránh phát sinh chi phí.

## Kiến trúc

{{< staticimg src="images/5-Workshop/smart-attendance-architecture.png" alt="Kiến trúc Smart Attendance SaaS" width="100%" >}}

Luồng MVP:

```text
React SPA → CloudFront → S3
     │
     └→ Cognito → JWT → API Gateway → Lambda → DynamoDB
                                           └→ CloudWatch
```

## Nội dung

1. [Giới thiệu giải pháp](5.1-introduction/)
2. [Chuẩn bị môi trường](5.2-prerequisites/)
3. [Phân tích kiến trúc](5.3-architecture/)
4. [Tạo xác thực Amazon Cognito](5.4-cognito/)
5. [Xây dựng Backend bằng AWS SAM](5.5-backend/)
6. [Triển khai và kiểm thử API](5.6-deploy-test/)
7. [Triển khai Frontend](5.7-frontend/)
8. [Giám sát và bảo mật](5.8-monitoring-security/)
9. [Dọn dẹp tài nguyên](5.9-cleanup/)

**Thời lượng:** khoảng 2–3 giờ.
