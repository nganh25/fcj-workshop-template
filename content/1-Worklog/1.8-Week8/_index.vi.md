---
title: "Worklog Tuần 8"
date: 2026-06-08
weight: 8
chapter: false
pre: "<b>1.8. </b>"
---

# Worklog Tuần 8

### Mục tiêu định hướng trong tuần 8:

- Triển khai lớp API và Compute cho Smart Attendance SaaS.
- Xây dựng Amazon API Gateway HTTP API v2.
- Cấu hình JWT Authorizer sử dụng Amazon Cognito.
- Phát triển các Lambda Function xử lý nghiệp vụ điểm danh.
- Triển khai chức năng Clock-in, Clock-out, quản lý hồ sơ và quản trị hệ thống.
- Kết nối Lambda với Amazon DynamoDB.
- Kiểm tra luồng xử lý đồng bộ từ Frontend đến API và cơ sở dữ liệu.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Khởi tạo Amazon API Gateway HTTP API v2; thiết kế các Route cho đăng nhập, hồ sơ người dùng, Clock-in, Clock-out, lịch sử điểm danh, báo cáo và quản trị. | 08/06/2026 | 08/06/2026 | Amazon API Gateway Documentation |
| Thứ Ba | Cấu hình JWT Authorizer cho API Gateway; kết nối với Cognito User Pool; kiểm tra yêu cầu có Token hợp lệ và yêu cầu không có Token. | 09/06/2026 | 09/06/2026 | API Gateway JWT Authorizer |
| Thứ Tư | Xây dựng Lambda Clock-in và Clock-out; xử lý thời gian điểm danh, thông tin người dùng, Tenant và kiểm tra bản ghi trùng lặp. | 10/06/2026 | 10/06/2026 | AWS Lambda Documentation |
| Thứ Năm | Xây dựng Lambda Attendance, Profile Update và Admin; xử lý truy vấn lịch sử điểm danh, cập nhật hồ sơ và các thao tác quản trị người dùng. | 11/06/2026 | 11/06/2026 | Node.js và AWS SDK Documentation |
| Thứ Sáu | Kết nối các Lambda Function với DynamoDB; kiểm thử toàn bộ luồng Frontend → CloudFront → API Gateway → Lambda → DynamoDB. | 12/06/2026 | 12/06/2026 | DynamoDB Developer Guide |

### Kết quả thu hoạch thực tế sau Tuần 8:

- Tạo thành công Amazon API Gateway HTTP API v2.
- Xây dựng các API Route chính cho hệ thống Smart Attendance SaaS.
- Cấu hình JWT Authorizer để xác thực Access Token được cấp bởi Amazon Cognito.
- Chặn được các yêu cầu không có Token hoặc sử dụng Token không hợp lệ.
- Phát triển Lambda Clock-in để ghi nhận thời gian bắt đầu làm việc.
- Phát triển Lambda Clock-out để ghi nhận thời gian kết thúc làm việc.
- Xây dựng Lambda Attendance để truy vấn và tổng hợp lịch sử điểm danh.
- Xây dựng Lambda Profile Update để cập nhật thông tin người dùng.
- Xây dựng Lambda Admin phục vụ các chức năng quản trị.
- Kết nối thành công Lambda với Amazon DynamoDB thông qua AWS SDK.
- Triển khai logic phân tách dữ liệu theo Tenant nhằm hỗ trợ mô hình SaaS đa tổ chức.
- Kiểm tra thành công luồng xử lý đồng bộ từ giao diện đến cơ sở dữ liệu.
- Hiểu rõ vai trò của API Gateway trong việc định tuyến, xác thực và chuyển tiếp yêu cầu đến các Lambda Function.