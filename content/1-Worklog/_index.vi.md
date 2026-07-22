---
title: "Nhật ký công việc"
date: 2026-04-20
weight: 1
chapter: false
pre: "<b>1. </b>"
---

# Nhật ký công việc

## Tổng quan quá trình thực tập

Trong quá trình tham gia chương trình **Workforce Bootcamp – First Cloud AI Journey**, tôi thực hiện kế hoạch học tập và phát triển dự án trong vòng **12 tuần**.

Bốn tuần đầu tập trung nghiên cứu kiến thức nền tảng về điện toán đám mây và các dịch vụ cốt lõi của Amazon Web Services. Nội dung bao gồm quản lý tài khoản và phân quyền IAM, dịch vụ máy chủ Amazon EC2, hệ thống mạng Amazon VPC, lưu trữ Amazon S3, cơ sở dữ liệu Amazon RDS và Amazon DynamoDB.

Từ tuần thứ năm, tôi tiếp tục tìm hiểu kiến trúc Serverless với AWS Lambda, Amazon API Gateway, Amazon Cognito và AWS SAM. Những kiến thức này được sử dụng để phân tích, thiết kế và triển khai dự án **Smart Attendance SaaS**.

Trong các tuần tiếp theo, nhóm tiến hành xây dựng giao diện React, Backend Node.js, hệ thống xác thực, chức năng check-in/check-out, quản lý dữ liệu điểm danh, xuất báo cáo, gửi email thông báo, bảo mật, CI/CD, giám sát và tối ưu hệ thống.

Vai trò chính của tôi trong dự án tập trung vào **Cloud Infrastructure và Cloud Networking**, bao gồm thiết kế kiến trúc AWS, cấu hình các dịch vụ Cloud, hỗ trợ triển khai Backend, kiểm tra luồng kết nối, bảo mật và giám sát hệ thống.

## Nội dung thực hiện theo từng tuần

### [Tuần 1 – Làm quen với AWS và chuẩn bị môi trường](1.1-week1/)

Làm quen với chương trình First Cloud Journey; tìm hiểu Cloud Computing, AWS Global Infrastructure, IAM, MFA, AWS Budgets và AWS Well-Architected Framework.

### [Tuần 2 – Dịch vụ Compute và giám sát hệ thống](1.2-week2/)

Tìm hiểu Amazon EC2, EBS, EFS, Auto Scaling, Elastic Load Balancing và Amazon CloudWatch; thực hành triển khai máy chủ Web trên AWS.

### [Tuần 3 – Hệ thống mạng trên AWS](1.3-week3/)

Tìm hiểu Amazon VPC, Public Subnet, Private Subnet, Route Table, Internet Gateway, NAT Gateway, Security Group, Network ACL và Route 53.

### [Tuần 4 – Lưu trữ và cơ sở dữ liệu](1.4-week4/)

Thực hành Amazon S3; tìm hiểu Amazon RDS và DynamoDB; so sánh cơ sở dữ liệu quan hệ với NoSQL và thiết kế dữ liệu ban đầu cho dự án.

### [Tuần 5 – Kiến trúc Serverless](1.5-week5/)

Tìm hiểu AWS Lambda, Amazon API Gateway, Amazon Cognito, AWS SAM và CloudFormation; xây dựng ứng dụng Serverless thử nghiệm.

### [Tuần 6 – Phân tích và thiết kế Smart Attendance SaaS](1.6-week6/)

Phân tích yêu cầu dự án, xác định người dùng, chức năng MVP, kiến trúc AWS, mô hình dữ liệu DynamoDB và phân chia nhiệm vụ trong nhóm.

### [Tuần 7 – Xây dựng Frontend và xác thực người dùng](1.7-week7/)

Phát triển React SPA, AuthContext, giao diện đăng nhập và Dashboard; tích hợp Amazon Cognito; triển khai Frontend trên Amazon S3 và CloudFront.

### [Tuần 8 – Phát triển API và chức năng điểm danh](1.8-week8/)

Triển khai API Gateway HTTP API, JWT Authorizer và các Lambda Function xử lý Clock-in, Clock-out, lịch sử điểm danh, hồ sơ và quản trị.

### [Tuần 9 – Báo cáo và xử lý sự kiện](1.9-week9/)

Hoàn thiện DynamoDB Single-Table Design; xây dựng chức năng xuất báo cáo; sử dụng Amazon S3, Step Functions, SQS, DynamoDB Streams, EventBridge và SES.

### [Tuần 10 – Bảo mật hệ thống](1.10-week10/)

Cấu hình AWS WAF, AWS Shield, Secrets Manager, AWS KMS và IAM Policy; kiểm tra phân tách dữ liệu Tenant và tìm hiểu Security Hub, GuardDuty.

### [Tuần 11 – CI/CD, kiểm thử và giám sát](1.11-week11/)

Hoàn thiện Infrastructure as Code; xây dựng CodePipeline và CodeBuild; thực hiện kiểm thử; thiết lập CloudWatch Logs, Dashboard, Alarm và AWS X-Ray.

### [Tuần 12 – Tối ưu và hoàn thiện dự án](1.12-week12/)

Rà soát kiến trúc, tối ưu hiệu năng và chi phí, hoàn thiện bảo mật, tài liệu kỹ thuật, Worklog, kịch bản Demo và bài thuyết trình cuối kỳ.

## Kết quả tổng quát

Sau 12 tuần thực hiện, nhóm đã xây dựng phiên bản MVP của hệ thống **Smart Attendance SaaS** theo kiến trúc Serverless và Event-Driven trên AWS.

Hệ thống hỗ trợ các chức năng chính như:

- Đăng ký, đăng nhập và phân quyền người dùng.
- Quản lý thông tin nhân viên và tổ chức.
- Thực hiện Clock-in và Clock-out.
- Theo dõi lịch sử điểm danh.
- Tổng hợp và xuất báo cáo.
- Lưu trữ báo cáo trên Amazon S3.
- Gửi email thông báo qua Amazon SES.
- Xử lý tác vụ bất đồng bộ bằng SQS và Step Functions.
- Giám sát hoạt động bằng CloudWatch và AWS X-Ray.
- Tự động hóa triển khai bằng CloudFormation, CodeBuild và CodePipeline.

Thông qua quá trình thực hiện, tôi nâng cao kiến thức về AWS Serverless, Cloud Infrastructure, Cloud Networking, Cloud Security, DevOps, CI/CD và kiến trúc Event-Driven. Đồng thời, tôi cải thiện kỹ năng làm việc nhóm, quản lý mã nguồn GitHub, xử lý lỗi và trình bày giải pháp kỹ thuật.