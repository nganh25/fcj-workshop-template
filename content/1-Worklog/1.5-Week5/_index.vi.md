---
title: "Worklog Tuần 5"
date: 2026-05-18
weight: 5
chapter: false
pre: "<b>1.5. </b>"
---

# Worklog Tuần 5

### Mục tiêu định hướng trong tuần 5:

- Tìm hiểu kiến trúc Serverless trên AWS.
- Nắm được cách hoạt động của AWS Lambda và Amazon API Gateway.
- Tìm hiểu cơ chế xác thực người dùng bằng Amazon Cognito.
- Thực hành xây dựng REST API đơn giản kết nối Lambda với DynamoDB.
- Làm quen với AWS Serverless Application Model và Infrastructure as Code.
- Đánh giá khả năng áp dụng kiến trúc Serverless cho Smart Attendance SaaS.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Tìm hiểu AWS Lambda, Lambda Function, Trigger, Execution Role, Environment Variable, Timeout và cơ chế tính chi phí. | 18/05/2026 | 18/05/2026 | AWS Lambda Documentation |
| Thứ Ba | Thực hành tạo Lambda Function bằng Node.js; xử lý Request, Response và kiểm tra Function bằng Test Event trên AWS Console. | 19/05/2026 | 19/05/2026 | AWS Lambda Workshop |
| Thứ Tư | Tìm hiểu Amazon API Gateway; tạo REST API, Method, Resource và tích hợp API Gateway với Lambda Function. | 20/05/2026 | 20/05/2026 | Amazon API Gateway Documentation |
| Thứ Năm | Tìm hiểu Amazon Cognito User Pool, quy trình đăng ký, đăng nhập, xác nhận tài khoản và sử dụng JWT Token để xác thực API. | 21/05/2026 | 21/05/2026 | Amazon Cognito Documentation |
| Thứ Sáu | Tìm hiểu AWS SAM và CloudFormation; xây dựng ứng dụng Serverless thử nghiệm gồm API Gateway, Lambda và DynamoDB; đánh giá khả năng áp dụng cho dự án. | 22/05/2026 | 22/05/2026 | AWS SAM Documentation |

### Kết quả thu hoạch thực tế sau Tuần 5:

- Hiểu được đặc điểm và lợi ích của kiến trúc Serverless trên AWS.
- Tạo và kiểm thử thành công Lambda Function bằng Node.js.
- Biết cách cấu hình IAM Execution Role cho Lambda theo nguyên tắc phân quyền tối thiểu.
- Tạo REST API bằng Amazon API Gateway và kết nối thành công với Lambda.
- Thực hiện thử nghiệm đọc và ghi dữ liệu từ Lambda vào DynamoDB.
- Hiểu quy trình đăng ký, đăng nhập và xác thực người dùng bằng Amazon Cognito.
- Nắm được vai trò của Access Token, ID Token và Refresh Token.
- Làm quen với AWS SAM Template và cách mô tả tài nguyên AWS bằng mã nguồn.
- Xác định kiến trúc Serverless là giải pháp phù hợp cho Smart Attendance SaaS nhờ khả năng tự động mở rộng, giảm công việc vận hành máy chủ và tối ưu chi phí cho giai đoạn đầu.