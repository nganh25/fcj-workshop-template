---
title: "Worklog Tuần 7"
date: 2026-06-01
weight: 7
chapter: false
pre: "<b>1.7. </b>"
---

# Worklog Tuần 7

### Mục tiêu định hướng trong tuần 7:

- Bắt đầu triển khai giao diện người dùng cho hệ thống Smart Attendance SaaS.
- Xây dựng giao diện React Single Page Application cho Manager, Admin và Employee.
- Tích hợp Amazon Cognito để thực hiện đăng ký, đăng nhập và xác thực người dùng.
- Xây dựng cơ chế lưu trữ và sử dụng JWT Token khi gọi API.
- Thiết lập Amazon S3 để lưu trữ mã nguồn Frontend dạng Static Website.
- Tìm hiểu cách phân phối giao diện thông qua Amazon CloudFront và Amazon Route 53.
- Kiểm tra luồng truy cập từ người dùng đến Frontend và hệ thống xác thực.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Phân tích giao diện cần thiết cho Manager, Admin và Employee; xây dựng cấu trúc thư mục Frontend React; xác định các trang đăng nhập, đăng ký, Dashboard, điểm danh, lịch sử và báo cáo. | 01/06/2026 | 01/06/2026 | Tài liệu thiết kế dự án |
| Thứ Ba | Xây dựng giao diện đăng ký, đăng nhập và quản lý trạng thái người dùng; tạo `AuthContext` để quản lý thông tin xác thực trong toàn bộ ứng dụng. | 02/06/2026 | 02/06/2026 | React Documentation |
| Thứ Tư | Tạo Amazon Cognito User Pool; cấu hình User Pool Client; thiết lập các thuộc tính người dùng, chính sách mật khẩu và quy trình xác nhận tài khoản. | 03/06/2026 | 03/06/2026 | Amazon Cognito Documentation |
| Thứ Năm | Tích hợp Frontend với Amazon Cognito; xử lý đăng ký, đăng nhập, đăng xuất và lưu JWT Token; kiểm tra khả năng phân quyền theo nhóm người dùng. | 04/06/2026 | 04/06/2026 | AWS Cognito Developer Guide |
| Thứ Sáu | Build React SPA; tạo S3 Bucket phục vụ Static Website Hosting; cấu hình CloudFront Distribution và kiểm tra luồng truy cập từ người dùng đến giao diện hệ thống. | 05/06/2026 | 05/06/2026 | Amazon S3 và CloudFront Documentation |

### Kết quả thu hoạch thực tế sau Tuần 7:

- Hoàn thành cấu trúc cơ bản của Frontend React cho Smart Attendance SaaS.
- Xây dựng được các trang đăng ký, đăng nhập, Dashboard và giao diện điểm danh.
- Tạo `AuthContext` để quản lý trạng thái đăng nhập và thông tin người dùng.
- Tích hợp thành công Amazon Cognito với ứng dụng React.
- Người dùng có thể đăng ký, xác nhận tài khoản, đăng nhập và đăng xuất.
- Frontend nhận được ID Token, Access Token và Refresh Token từ Cognito.
- Xây dựng cơ chế đính kèm JWT Token vào Header khi gửi yêu cầu đến API Gateway.
- Build thành công ứng dụng React thành các tệp tĩnh.
- Tạo S3 Bucket để lưu trữ và phân phối Frontend.
- Hiểu được luồng truy cập: người dùng gửi yêu cầu đến Route 53, CloudFront phân phối nội dung và lấy Static Content từ Amazon S3.
- Hoàn thành bước đầu của lớp Global Edge Services và Authentication trong kiến trúc dự án.