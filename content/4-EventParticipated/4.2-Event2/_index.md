---
title: "Event 2 – Smart Attendance SaaS Final Demo & Technical Review"
date: 2026-07-10
weight: 2
chapter: false
pre: "<b>4.2. </b>"
---

# Smart Attendance SaaS Final Demo & Technical Review

## Thông tin sự kiện

- **Thời gian:** 10/07/2026
- **Hình thức:** Trình bày và review dự án cuối chương trình
- **Vai trò:** Thành viên nhóm, phụ trách nội dung Cloud Infrastructure và Cloud Networking

## Nội dung trình bày

- Bài toán và phạm vi MVP.
- Kiến trúc Route 53, CloudFront, S3, Cognito, API Gateway, Lambda và DynamoDB.
- Luồng Clock-in, Clock-out, Attendance, Profile và Report.
- Xử lý bất đồng bộ với Step Functions, SQS và DLQ.
- Gửi email qua EventBridge, Lambda Worker và SES.
- Bảo mật với WAF, Shield, IAM, KMS và Secrets Manager.
- CI/CD bằng CloudFormation, CodeBuild và CodePipeline.
- Giám sát bằng CloudWatch và X-Ray.

## Phần công việc của tôi

- Giải thích luồng kết nối giữa các lớp Edge, Authentication, API, Compute và Data.
- Rà soát IAM Role, Security Configuration và Tenant Isolation.
- Hỗ trợ triển khai, kiểm thử API và xử lý lỗi môi trường.
- Chuẩn bị sơ đồ kiến trúc và nội dung kỹ thuật cho phần trình bày.

## Bài học

- Kiến trúc cần cân bằng giữa tính đầy đủ và phạm vi MVP.
- Demo phải có kịch bản rõ ràng và dữ liệu kiểm thử ổn định.
- Monitoring và log là thành phần cần thiết, không phải phần bổ sung sau cùng.
- Tài liệu kiến trúc giúp nhóm thống nhất cách hiểu và giảm lỗi tích hợp.
