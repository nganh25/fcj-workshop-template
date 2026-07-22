---
title: "Worklog Tuần 12"
date: 2026-07-06
weight: 12
chapter: false
pre: "<b>1.12. </b>"
---

# Worklog Tuần 12

### Mục tiêu định hướng trong tuần 12:

- Rà soát và hoàn thiện toàn bộ dự án Smart Attendance SaaS.
- Kiểm tra tính ổn định, bảo mật và khả năng mở rộng của hệ thống.
- Tối ưu hiệu năng và chi phí các dịch vụ AWS.
- Hoàn thiện tài liệu kiến trúc và tài liệu hướng dẫn triển khai.
- Chuẩn bị kịch bản Demo và bài thuyết trình dự án.
- Đánh giá kết quả đạt được so với mục tiêu ban đầu.
- Tổng kết kiến thức và kinh nghiệm trong chương trình First Cloud Journey.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Kiểm tra toàn bộ kiến trúc; xác minh kết nối giữa Route 53, CloudFront, S3, Cognito, API Gateway, Lambda, DynamoDB và các dịch vụ xử lý sự kiện. | 06/07/2026 | 06/07/2026 | Sơ đồ kiến trúc dự án |
| Thứ Ba | Tối ưu Lambda Memory, Timeout và Dependency; rà soát DynamoDB Capacity, S3 Lifecycle, CloudFront Cache và số lượng Log lưu trữ. | 07/07/2026 | 07/07/2026 | AWS Cost Optimization |
| Thứ Tư | Kiểm tra bảo mật lần cuối; rà soát AWS WAF, Secrets Manager, KMS, IAM Policy, Tenant Isolation, Security Hub và GuardDuty. | 08/07/2026 | 08/07/2026 | AWS Security Best Practices |
| Thứ Năm | Hoàn thiện tài liệu dự án, sơ đồ kiến trúc, mô tả luồng xử lý, hướng dẫn triển khai, báo cáo thực tập và nội dung Worklog trên website Hugo. | 09/07/2026 | 09/07/2026 | Tài liệu dự án của nhóm |
| Thứ Sáu | Chuẩn bị và thực hiện Demo; trình bày kiến trúc, chức năng chính, kết quả đạt được, khó khăn, giải pháp và định hướng phát triển trong tương lai. | 10/07/2026 | 10/07/2026 | Nội dung thuyết trình dự án |

### Kết quả thu hoạch thực tế sau Tuần 12:

- Hoàn thành phiên bản MVP của hệ thống Smart Attendance SaaS.
- Hoàn thiện giao diện React SPA dành cho người quản lý và nhân viên.
- Hoàn thiện chức năng đăng ký, đăng nhập và xác thực bằng Amazon Cognito.
- Hoàn thiện API Clock-in, Clock-out, Attendance, Report, Profile và Admin.
- Triển khai Backend theo kiến trúc Serverless bằng API Gateway và AWS Lambda.
- Lưu trữ dữ liệu theo mô hình Single-Table Design trên Amazon DynamoDB.
- Áp dụng Tenant Isolation để hỗ trợ nhiều tổ chức sử dụng chung hệ thống.
- Hoàn thiện chức năng xuất báo cáo và lưu trữ tệp trên Amazon S3.
- Triển khai xử lý bất đồng bộ bằng Step Functions, SQS và Dead-Letter Queue.
- Xây dựng luồng Event-Driven bằng DynamoDB Streams, EventBridge, Lambda và Amazon SES.
- Phân phối Frontend bằng Amazon CloudFront và Amazon S3.
- Bảo vệ hệ thống bằng AWS WAF, AWS Shield, Cognito, IAM, Secrets Manager và AWS KMS.
- Thiết lập CI/CD bằng CloudFormation, CodePipeline và CodeBuild.
- Thiết lập Logging, Monitoring và Tracing bằng CloudWatch và AWS X-Ray.
- Rà soát chi phí và tối ưu cấu hình tài nguyên theo nhu cầu thực tế.
- Hoàn thiện tài liệu kỹ thuật, sơ đồ kiến trúc, báo cáo và nội dung Workshop.
- Nâng cao kiến thức về AWS Serverless, Cloud Infrastructure, Cloud Security, DevOps và kiến trúc Event-Driven.
- Cải thiện kỹ năng làm việc nhóm, quản lý mã nguồn, xử lý lỗi và trình bày dự án.
- Hoàn thành mục tiêu của chương trình Workforce Bootcamp – First Cloud Journey.

### Kiến trúc tổng thể của Smart Attendance SaaS

![Kiến trúc Smart Attendance SaaS](/images/architecture/smart-attendance-architecture.jpg)

Sơ đồ kiến trúc thể hiện hệ thống Smart Attendance SaaS được xây dựng theo mô hình Serverless và Event-Driven trên AWS. Giao diện React SPA được lưu trữ trên Amazon S3 và phân phối thông qua Amazon CloudFront. Amazon Route 53 thực hiện phân giải tên miền, trong khi AWS WAF và AWS Shield bảo vệ ứng dụng tại lớp biên.

Người dùng được xác thực bằng Amazon Cognito. Các yêu cầu có JWT Token hợp lệ được gửi đến Amazon API Gateway và chuyển tiếp đến các AWS Lambda Function xử lý nghiệp vụ như Clock-in, Clock-out, Attendance, Report, Admin và Subscription.

Dữ liệu được lưu trữ trong Amazon DynamoDB theo mô hình Single-Table Design và được mã hóa bằng AWS KMS. Các tác vụ cần nhiều thời gian được xử lý bất đồng bộ thông qua AWS Step Functions, Amazon SQS và Dead-Letter Queue. DynamoDB Streams, Amazon EventBridge, Lambda Email Worker và Amazon SES được sử dụng để xây dựng luồng thông báo theo kiến trúc Event-Driven.

Hệ thống được triển khai tự động bằng AWS CloudFormation, CodePipeline và CodeBuild. Amazon CloudWatch và AWS X-Ray hỗ trợ ghi log, giám sát, cảnh báo và truy vết yêu cầu trong toàn bộ ứng dụng.