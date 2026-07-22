---
title: "Worklog Tuần 10"
date: 2026-06-22
weight: 10
chapter: false
pre: "<b>1.10. </b>"
---

# Worklog Tuần 10

### Mục tiêu định hướng trong tuần 10:

- Tăng cường bảo mật cho toàn bộ hệ thống Smart Attendance SaaS.
- Cấu hình AWS WAF và AWS Shield cho CloudFront.
- Quản lý thông tin nhạy cảm bằng AWS Secrets Manager.
- Mã hóa dữ liệu bằng AWS Key Management Service.
- Rà soát IAM Role và IAM Policy của các Lambda Function.
- Bảo vệ dữ liệu của từng Tenant trong mô hình SaaS.
- Tìm hiểu AWS Security Hub và Amazon GuardDuty.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Cấu hình AWS WAF cho CloudFront Distribution; tạo Rule giới hạn Request, chặn Bot và kiểm soát truy cập theo vị trí địa lý. | 22/06/2026 | 22/06/2026 | AWS WAF Documentation |
| Thứ Ba | Tìm hiểu AWS Shield Standard và Shield Advanced; đánh giá khả năng bảo vệ hệ thống trước các cuộc tấn công DDoS. | 23/06/2026 | 23/06/2026 | AWS Shield Documentation |
| Thứ Tư | Sử dụng AWS Secrets Manager để lưu API Key, Credential và thông tin tích hợp; cấu hình Lambda truy cập Secret thông qua IAM Role. | 24/06/2026 | 24/06/2026 | AWS Secrets Manager Documentation |
| Thứ Năm | Tạo AWS KMS Customer Managed Key; sử dụng khóa KMS để mã hóa dữ liệu DynamoDB, S3 và các thông tin nhạy cảm. | 25/06/2026 | 25/06/2026 | AWS KMS Documentation |
| Thứ Sáu | Rà soát IAM Policy, phân quyền theo Least Privilege; kiểm tra Tenant Isolation; tìm hiểu Security Hub và GuardDuty để phát hiện rủi ro bảo mật. | 26/06/2026 | 26/06/2026 | AWS Security Best Practices |

### Kết quả thu hoạch thực tế sau Tuần 10:

- Cấu hình AWS WAF cho CloudFront Distribution của hệ thống.
- Thiết lập Rate-Based Rule nhằm giới hạn số lượng yêu cầu bất thường.
- Tìm hiểu cách chặn Bot và kiểm soát truy cập theo khu vực địa lý.
- Hiểu vai trò của AWS Shield trong việc giảm thiểu các cuộc tấn công DDoS.
- Chuyển các thông tin nhạy cảm ra khỏi mã nguồn và lưu trữ trong AWS Secrets Manager.
- Cấu hình Lambda lấy Secret tại thời điểm thực thi.
- Tạo Customer Managed Key trên AWS KMS.
- Áp dụng mã hóa cho dữ liệu lưu trong DynamoDB và Amazon S3.
- Rà soát và thu hẹp quyền truy cập của các IAM Role.
- Áp dụng nguyên tắc Least Privilege cho Lambda, API Gateway và các dịch vụ liên quan.
- Kiểm tra logic Tenant Isolation để người dùng không thể truy cập dữ liệu của tổ chức khác.
- Tìm hiểu cách AWS Security Hub tổng hợp trạng thái bảo mật của tài khoản.
- Tìm hiểu Amazon GuardDuty trong việc phát hiện hành vi bất thường và các mối đe dọa.
- Hoàn thiện lớp bảo mật phù hợp với kiến trúc AWS Well-Architected Framework.