---
title: "Worklog Tuần 11"
date: 2026-06-29
weight: 11
chapter: false
pre: "<b>1.11. </b>"
---

# Worklog Tuần 11

### Mục tiêu định hướng trong tuần 11:

- Tự động hóa quá trình Build, Test và Deploy.
- Hoàn thiện Infrastructure as Code bằng AWS CloudFormation hoặc AWS SAM.
- Xây dựng CI/CD Pipeline bằng AWS CodePipeline và AWS CodeBuild.
- Thực hiện Unit Test và Integration Test.
- Thiết lập hệ thống giám sát bằng Amazon CloudWatch.
- Tìm hiểu AWS X-Ray để truy vết yêu cầu phân tán.
- Phát hiện và xử lý các lỗi trong toàn bộ hệ thống.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Hoàn thiện `template.yaml`; khai báo API Gateway, Lambda, DynamoDB, SQS, Step Functions, S3 và các IAM Role bằng Infrastructure as Code. | 29/06/2026 | 29/06/2026 | AWS SAM và CloudFormation Documentation |
| Thứ Ba | Tạo `buildspec.yml`; cấu hình AWS CodeBuild để cài đặt Dependency, chạy Test, Build và đóng gói ứng dụng Backend. | 30/06/2026 | 30/06/2026 | AWS CodeBuild Documentation |
| Thứ Tư | Xây dựng AWS CodePipeline; kết nối Source Repository, CodeBuild và bước Deploy để tự động triển khai khi có thay đổi mã nguồn. | 01/07/2026 | 01/07/2026 | AWS CodePipeline Documentation |
| Thứ Năm | Thực hiện Unit Test, Integration Test và kiểm thử luồng đăng nhập, Clock-in, Clock-out, báo cáo, Email và phân quyền người dùng. | 02/07/2026 | 02/07/2026 | Tài liệu kiểm thử dự án |
| Thứ Sáu | Thiết lập CloudWatch Logs, Metrics, Alarm và Dashboard; bật AWS X-Ray cho API Gateway và Lambda; phân tích và sửa các lỗi phát sinh. | 03/07/2026 | 03/07/2026 | CloudWatch và AWS X-Ray Documentation |

### Kết quả thu hoạch thực tế sau Tuần 11:

- Hoàn thiện tệp `template.yaml` để mô tả hạ tầng Backend bằng mã nguồn.
- Có thể tạo hoặc cập nhật tài nguyên AWS theo quy trình nhất quán.
- Xây dựng `buildspec.yml` cho quá trình Build và Test tự động.
- Thiết lập AWS CodeBuild để cài đặt Package, kiểm thử và đóng gói ứng dụng.
- Tạo CI/CD Pipeline gồm Source, Build và Deploy.
- Giảm các thao tác triển khai thủ công và hạn chế sai sót giữa các môi trường.
- Thực hiện Unit Test cho các Lambda Function quan trọng.
- Thực hiện Integration Test cho luồng API Gateway, Lambda, DynamoDB, SQS và SES.
- Kiểm tra chức năng đăng nhập, Clock-in, Clock-out, quản lý hồ sơ và xuất báo cáo.
- Thiết lập CloudWatch Logs cho các Lambda Function.
- Xây dựng CloudWatch Dashboard theo dõi Request, Error, Duration và Throttle.
- Tạo CloudWatch Alarm khi tỷ lệ lỗi hoặc thời gian phản hồi vượt ngưỡng.
- Sử dụng AWS X-Ray để theo dõi đường đi của Request qua nhiều dịch vụ.
- Phát hiện và sửa lỗi phân quyền IAM, cấu hình CORS, JWT Token và truy vấn DynamoDB.