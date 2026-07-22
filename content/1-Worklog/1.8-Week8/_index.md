---
title: "Worklog Tuần 8"
date: 2026-06-08
weight: 8
chapter: false
pre: "<b>1.8. </b>"
---

# Worklog Tuần 8

### Mục tiêu định hướng trong tuần 8

- Triển khai API Gateway HTTP API v2.
- Cấu hình Cognito JWT Authorizer.
- Phát triển các Lambda nghiệp vụ.
- Kết nối Backend với DynamoDB.

### Nhiệm vụ cụ thể

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Thiết kế route cho Clock-in, Clock-out, Attendance, Profile và Admin. | 08/06/2026 | 08/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Ba | Cấu hình JWT Authorizer và CORS. | 09/06/2026 | 09/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Tư | Xây dựng Lambda Clock-in và chống ghi trùng. | 10/06/2026 | 10/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Năm | Xây dựng Clock-out, Attendance và Profile Update. | 11/06/2026 | 11/06/2026 | Tài liệu AWS và tài liệu dự án |
| Thứ Sáu | Kiểm thử luồng Frontend → API Gateway → Lambda → DynamoDB. | 12/06/2026 | 12/06/2026 | Tài liệu AWS và tài liệu dự án |

### Kết quả thu hoạch sau Tuần 8

- Các API nghiệp vụ hoạt động.
- Request không có token bị chặn.
- Dữ liệu được ghi và truy vấn theo Tenant.
- Hoàn thành luồng điểm danh đồng bộ.
