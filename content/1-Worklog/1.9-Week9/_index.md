---
title: "Worklog Tuần 9"
date: 2026-06-15
weight: 9
chapter: false
pre: "<b>1.9. </b>"
---

# Worklog Tuần 9

### Mục tiêu định hướng trong tuần 9

- Hoàn thiện DynamoDB Single-Table Design.
- Xây dựng chức năng xuất báo cáo.
- Triển khai xử lý bất đồng bộ.
- Tạo luồng gửi email theo sự kiện.

### Nhiệm vụ cụ thể

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Tối ưu PK/SK và truy vấn dữ liệu điểm danh. | 15/06/2026 | 15/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Ba | Xây dựng Lambda Report và Export CSV/Excel. | 16/06/2026 | 16/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Tư | Lưu báo cáo vào S3 theo Tenant. | 17/06/2026 | 17/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Năm | Tạo Step Functions, SQS và DLQ cho tác vụ dài. | 18/06/2026 | 18/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Sáu | Kết nối DynamoDB Streams, EventBridge, Email Worker và SES. | 19/06/2026 | 19/06/2026 | Tài liệu AWS và tài liệu dự án |

### Kết quả thu hoạch sau Tuần 9

- Hoàn thành báo cáo và lưu trữ S3.
- Hiểu retry và Dead-Letter Queue.
- Xây dựng luồng Event-Driven cơ bản.
- Phân biệt xử lý đồng bộ và bất đồng bộ.
