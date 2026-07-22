---
title: "Worklog Tuần 6"
date: 2026-05-25
weight: 6
chapter: false
pre: "<b>1.6. </b>"
---

# Worklog Tuần 6

### Mục tiêu định hướng trong tuần 6:

- Bắt đầu giai đoạn xây dựng dự án Smart Attendance SaaS.
- Phân tích bài toán điểm danh và xác định phạm vi của phiên bản MVP.
- Xác định các nhóm người dùng và chức năng chính của hệ thống.
- Thiết kế kiến trúc tổng thể trên AWS.
- Thiết kế cơ sở dữ liệu DynamoDB và các luồng xử lý chính.
- Phân chia nhiệm vụ cho các thành viên trong nhóm.
- Xây dựng kế hoạch phát triển và quản lý mã nguồn trên GitHub.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Thảo luận ý tưởng dự án Smart Attendance SaaS; xác định vấn đề cần giải quyết, đối tượng sử dụng và giá trị hệ thống mang lại. | 25/05/2026 | 25/05/2026 | Tài liệu dự án của nhóm |
| Thứ Ba | Phân tích yêu cầu chức năng cho Admin, Lecturer và Student; xác định các chức năng đăng ký, đăng nhập, quản lý lớp, tạo phiên điểm danh, check-in và xem báo cáo. | 26/05/2026 | 26/05/2026 | Biên bản họp nhóm |
| Thứ Tư | Thiết kế kiến trúc tổng thể gồm Frontend, Amazon Cognito, API Gateway, Lambda, DynamoDB, S3, CloudFront và Route 53. | 27/05/2026 | 27/05/2026 | AWS Architecture Center |
| Thứ Năm | Thiết kế cấu trúc dữ liệu cho User, Tenant, Class, Attendance Session và Attendance Record; xác định Partition Key, Sort Key và các chỉ mục cần thiết. | 28/05/2026 | 28/05/2026 | DynamoDB Data Modeling |
| Thứ Sáu | Phân chia nhiệm vụ giữa năm thành viên; tạo Repository GitHub, thiết lập Branch, quy tắc Pull Request, CODEOWNERS và xây dựng kế hoạch thực hiện theo từng giai đoạn. | 29/05/2026 | 29/05/2026 | GitHub Documentation |

### Kết quả thu hoạch thực tế sau Tuần 6:

- Xác định được mục tiêu của Smart Attendance SaaS là xây dựng hệ thống điểm danh trực tuyến có khả năng quản lý nhiều tổ chức hoặc đơn vị sử dụng.
- Xác định ba nhóm người dùng chính gồm Admin, Lecturer và Student.
- Hoàn thành danh sách chức năng chính cho phiên bản MVP.
- Xây dựng kiến trúc tổng thể theo mô hình Serverless trên AWS.
- Lựa chọn React cho Frontend và Node.js cho các Lambda Function.
- Lựa chọn Amazon Cognito để quản lý tài khoản và xác thực người dùng.
- Lựa chọn Amazon API Gateway làm cổng tiếp nhận yêu cầu từ Frontend.
- Lựa chọn AWS Lambda để xử lý nghiệp vụ và DynamoDB để lưu trữ dữ liệu.
- Thiết kế sơ bộ các bảng và thuộc tính cần thiết cho hệ thống.
- Hoàn thành việc phân chia nhiệm vụ cho năm thành viên trong nhóm.
- Thiết lập Repository GitHub và thống nhất quy trình làm việc thông qua Branch, Commit, Pull Request và Code Review.
- Xác định vai trò cá nhân tập trung vào Cloud Infrastructure, Cloud Networking, cấu hình dịch vụ AWS và hỗ trợ triển khai hệ thống.