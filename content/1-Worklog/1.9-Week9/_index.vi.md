---
title: "Worklog Tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: "<b>1.9. </b>"
---

# Worklog Tuần 9

### Mục tiêu định hướng trong tuần 9:

- Hoàn thiện lớp dữ liệu của Smart Attendance SaaS.
- Áp dụng Single-Table Design trên Amazon DynamoDB.
- Xây dựng chức năng báo cáo và xuất dữ liệu.
- Lưu trữ tệp báo cáo trên Amazon S3.
- Triển khai quy trình xử lý bất đồng bộ bằng AWS Step Functions và Amazon SQS.
- Tìm hiểu DynamoDB Streams và kiến trúc Event-Driven.
- Xây dựng hệ thống gửi thông báo qua Amazon SES.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Hoàn thiện thiết kế DynamoDB theo mô hình Single-Table Design; tổ chức dữ liệu Tenant, User, Attendance và Report bằng Partition Key và Sort Key. | 15/06/2026 | 15/06/2026 | DynamoDB Data Modeling |
| Thứ Ba | Xây dựng Lambda Report và Report Export; thực hiện truy vấn dữ liệu điểm danh, tổng hợp kết quả và xuất tệp CSV hoặc Excel. | 16/06/2026 | 16/06/2026 | AWS Lambda và Node.js Documentation |
| Thứ Tư | Tạo S3 Bucket lưu trữ báo cáo; thiết lập đường dẫn lưu tệp theo Tenant và thời gian; tìm hiểu S3 Intelligent-Tiering để tối ưu chi phí lưu trữ. | 17/06/2026 | 17/06/2026 | Amazon S3 Documentation |
| Thứ Năm | Thiết kế quy trình xử lý báo cáo bất đồng bộ bằng AWS Step Functions và Amazon SQS; cấu hình Dead-Letter Queue để lưu các yêu cầu xử lý thất bại. | 18/06/2026 | 18/06/2026 | Step Functions và Amazon SQS Documentation |
| Thứ Sáu | Kích hoạt DynamoDB Streams; kết nối EventBridge, SQS Email Queue, Lambda Email Worker và Amazon SES để gửi thông báo báo cáo hoặc trạng thái điểm danh. | 19/06/2026 | 19/06/2026 | EventBridge và Amazon SES Documentation |

### Kết quả thu hoạch thực tế sau Tuần 9:

- Hoàn thiện mô hình dữ liệu Single-Table Design trên Amazon DynamoDB.
- Dữ liệu của từng tổ chức được phân tách thông qua Tenant ID.
- Tối ưu truy vấn bằng cách lựa chọn Partition Key, Sort Key và các chỉ mục phù hợp.
- Xây dựng Lambda Report để truy vấn và tổng hợp dữ liệu điểm danh.
- Phát triển chức năng xuất báo cáo dưới định dạng CSV hoặc Excel.
- Lưu trữ tệp báo cáo trên Amazon S3 theo từng Tenant.
- Hiểu cách sử dụng S3 Intelligent-Tiering để tự động tối ưu chi phí lưu trữ.
- Xây dựng luồng xử lý báo cáo bất đồng bộ bằng Step Functions.
- Sử dụng Amazon SQS để tiếp nhận và điều phối các yêu cầu xuất báo cáo.
- Cấu hình Dead-Letter Queue để hạn chế mất dữ liệu khi xử lý lỗi.
- Kích hoạt DynamoDB Streams để ghi nhận sự thay đổi dữ liệu.
- Sử dụng EventBridge để định tuyến sự kiện đến hệ thống thông báo.
- Xây dựng Lambda Email Worker để nhận Message từ SQS và gửi Email thông qua Amazon SES.
- Hiểu được sự khác nhau giữa xử lý đồng bộ và xử lý bất đồng bộ trong hệ thống Cloud.