---
title: "Báo cáo thực tập"
chapter: false
---

# BÁO CÁO THỰC TẬP

## Thông tin sinh viên

**Họ và tên:** Nguyễn Trung Anh  
**Số điện thoại:** 0378477079  
**Email:** [nguyentrunganh677@gmail.com](mailto:nguyentrunganh677@gmail.com)  
**Trường:** Đại học Công nghệ TP. Hồ Chí Minh (HUTECH)  
**Ngành:** Công nghệ thông tin  
**Lớp:** 22DTHJB2  
**Đơn vị thực tập:** Công ty TNHH Amazon Web Services Việt Nam  
**Chương trình:** Workforce Bootcamp – First Cloud AI Journey  
**Thời gian thực tập:** 17/04/2026 – 10/07/2026  

---

## Ảnh đại diện

{{< avatar src="images/avatar.jpg" alt="Ảnh đại diện Nguyễn Trung Anh" >}}

---

## Đề tài thực tập

### Smart Attendance SaaS

**Smart Attendance SaaS** là hệ thống quản lý chấm công đa tổ chức được xây dựng trên nền tảng Amazon Web Services theo kiến trúc **Serverless** và **Event-Driven**.

Trong giai đoạn học tập, tôi đã tìm hiểu các dịch vụ AWS nền tảng như IAM, EC2, VPC, Amazon S3, Amazon CloudFront, Amazon Route 53, Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon SQS, AWS Step Functions, Amazon EventBridge, Amazon SES, Amazon CloudWatch và các công cụ Infrastructure as Code.

Trong giai đoạn thực hiện dự án, nhóm tiến hành phân tích, thiết kế và triển khai hệ thống cho phép người dùng đăng nhập, thực hiện Clock-in và Clock-out, quản lý hồ sơ, xem lịch sử điểm danh, xuất báo cáo, nhận thông báo và theo dõi trạng thái hoạt động của hệ thống.

Vai trò chính của tôi tập trung vào **Cloud Infrastructure, Cloud Networking, triển khai các dịch vụ AWS, bảo mật, giám sát hệ thống và hỗ trợ quy trình DevOps**.

---

## Kiến trúc tổng quan

Hệ thống Smart Attendance SaaS được xây dựng với các thành phần chính:

- **Amazon Route 53:** quản lý DNS và tên miền.
- **Amazon CloudFront:** phân phối nội dung Frontend.
- **Amazon S3:** lưu trữ React SPA và các file báo cáo.
- **Amazon Cognito:** xác thực người dùng và phát hành JWT.
- **Amazon API Gateway:** cung cấp các API cho Frontend.
- **AWS Lambda:** xử lý nghiệp vụ của hệ thống.
- **Amazon DynamoDB:** lưu trữ dữ liệu người dùng và điểm danh.
- **AWS Step Functions và Amazon SQS:** xử lý các tác vụ bất đồng bộ.
- **Amazon EventBridge và Amazon SES:** xử lý sự kiện và gửi email.
- **Amazon CloudWatch:** thu thập log, metric và cảnh báo.
- **AWS CloudFormation và AWS SAM:** quản lý hạ tầng bằng mã nguồn.

---

## Nội dung báo cáo

1. **Nhật ký công việc:** Trình bày tiến độ học tập và phát triển dự án trong 12 tuần.
2. **Bản đề xuất:** Mô tả mục tiêu, phạm vi và kiến trúc của Smart Attendance SaaS.
3. **Các bài blog đã đăng:** Tổng hợp các bài viết về AWS Security, AWS Backup và Cost Optimization.
4. **Các sự kiện đã tham gia:** Trình bày nội dung và bài học từ các hoạt động chuyên môn.
5. **Workshop:** Hướng dẫn xây dựng và triển khai Smart Attendance SaaS trên AWS.
6. **Tự đánh giá:** Đánh giá kiến thức, kỹ năng và những điểm cần cải thiện.
7. **Chia sẻ, đóng góp ý kiến:** Nhận xét và đề xuất dành cho chương trình thực tập.

---

## Kết quả đạt được

Thông qua quá trình học tập và thực hiện dự án, tôi đã:

- Hiểu rõ hơn về kiến trúc Cloud và các dịch vụ cốt lõi trên AWS.
- Biết cách thiết kế hệ thống theo mô hình Serverless và Event-Driven.
- Thực hành triển khai Frontend, Backend, Database và hệ thống xác thực trên AWS.
- Áp dụng Infrastructure as Code trong quá trình triển khai hạ tầng.
- Nâng cao kỹ năng bảo mật, giám sát và xử lý sự cố.
- Cải thiện kỹ năng làm việc nhóm, sử dụng GitHub và viết tài liệu kỹ thuật.
- Có thêm kinh nghiệm thực tế cho định hướng Cloud Engineer và Infrastructure Engineer.