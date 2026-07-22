---
title: "Worklog Tuần 4"
date: 2026-05-11
weight: 4
chapter: false
pre: "<b>1.4. </b>"
---

# Worklog Tuần 4

### Mục tiêu định hướng trong tuần 4:

- Tìm hiểu các dịch vụ lưu trữ và cơ sở dữ liệu phổ biến trên AWS.
- Nắm được cách hoạt động của Amazon S3, Amazon RDS và Amazon DynamoDB.
- Thực hành tạo Bucket, quản lý Object và cấu hình quyền truy cập trên Amazon S3.
- Tìm hiểu mô hình cơ sở dữ liệu quan hệ và cơ sở dữ liệu NoSQL.
- So sánh Amazon RDS với Amazon DynamoDB để lựa chọn giải pháp phù hợp cho dự án Smart Attendance SaaS.
- Tìm hiểu cơ chế sao lưu, mã hóa và bảo vệ dữ liệu trên AWS.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Tìm hiểu Amazon S3, cấu trúc Bucket và Object, các lớp lưu trữ, tính năng Versioning và Lifecycle Policy. | 11/05/2026 | 11/05/2026 | Amazon S3 Documentation |
| Thứ Ba | Thực hành tạo S3 Bucket, tải tệp lên Bucket, cấu hình quyền truy cập, Bucket Policy, mã hóa và kiểm tra khả năng truy xuất dữ liệu. | 12/05/2026 | 12/05/2026 | AWS S3 Workshop |
| Thứ Tư | Tìm hiểu Amazon RDS, các hệ quản trị cơ sở dữ liệu được hỗ trợ, Multi-AZ, Read Replica, Backup và Snapshot. | 13/05/2026 | 13/05/2026 | Amazon RDS Documentation |
| Thứ Năm | Tìm hiểu Amazon DynamoDB, Partition Key, Sort Key, Global Secondary Index, cơ chế đọc ghi và khả năng tự động mở rộng. | 14/05/2026 | 14/05/2026 | Amazon DynamoDB Documentation |
| Thứ Sáu | So sánh Amazon RDS và DynamoDB; thiết kế mô hình dữ liệu ban đầu cho User, Class, Attendance Session và Attendance Record của Smart Attendance SaaS. | 15/05/2026 | 15/05/2026 | AWS Database Documentation |

### Kết quả thu hoạch thực tế sau Tuần 4:

- Hiểu được cách Amazon S3 lưu trữ và quản lý dữ liệu dưới dạng Object.
- Tạo và cấu hình thành công S3 Bucket phục vụ việc lưu trữ tệp thử nghiệm.
- Biết cách sử dụng Bucket Policy, Versioning, Lifecycle Policy và mã hóa dữ liệu.
- Nắm được đặc điểm của cơ sở dữ liệu quan hệ trên Amazon RDS.
- Hiểu vai trò của Multi-AZ, Read Replica, Snapshot và cơ chế sao lưu tự động.
- Hiểu được mô hình NoSQL của Amazon DynamoDB và cách lựa chọn Partition Key, Sort Key.
- Phân biệt được trường hợp sử dụng Amazon RDS và Amazon DynamoDB.
- Lựa chọn DynamoDB làm cơ sở dữ liệu chính cho Smart Attendance SaaS vì phù hợp với kiến trúc Serverless, khả năng mở rộng tự động và mô hình thanh toán theo nhu cầu sử dụng.
- Hoàn thành mô hình dữ liệu sơ bộ cho người dùng, lớp học, phiên điểm danh và lịch sử điểm danh.