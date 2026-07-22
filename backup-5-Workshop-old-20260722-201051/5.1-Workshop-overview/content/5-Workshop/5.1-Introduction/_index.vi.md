---
title: "Giới thiệu giải pháp"
date: 2026-07-22
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---

# Giới thiệu giải pháp

## Bài toán

Hệ thống cần ghi nhận thời gian bắt đầu và kết thúc làm việc của nhân viên, đồng thời bảo đảm mỗi người chỉ thao tác trên dữ liệu của chính mình.

## API của MVP

| Phương thức | Endpoint | Chức năng |
|---|---|---|
| `GET` | `/health` | Kiểm tra API, không cần đăng nhập |
| `POST` | `/attendance/checkin` | Tạo bản ghi check-in trong ngày |
| `POST` | `/attendance/checkout` | Cập nhật thời gian check-out |
| `GET` | `/attendance/history` | Xem tối đa 31 bản ghi gần nhất |

## Dịch vụ AWS

- **Amazon Cognito:** đăng ký, đăng nhập và phát hành JWT.
- **API Gateway HTTP API:** endpoint và JWT Authorizer.
- **AWS Lambda:** xử lý nghiệp vụ.
- **Amazon DynamoDB:** lưu bản ghi điểm danh.
- **AWS SAM:** Infrastructure as Code.
- **CloudWatch:** log, metric và alarm.
- **S3 + CloudFront:** phân phối React SPA.

## Kết quả

CloudFormation trả về `ApiUrl`, `UserPoolId`, `UserPoolClientId` và `AttendanceTableName`. Sau đó bạn tạo người dùng thử, nhận Access Token và gọi API.
