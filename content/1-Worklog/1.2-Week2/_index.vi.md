---
title: "Worklog Tuần 2"
date: 2026-04-27
weight: 2
chapter: false
pre: "<b>1.2. </b>"
---

# Worklog Tuần 2

### Mục tiêu định hướng trong tuần 2:

- Tìm hiểu nhóm dịch vụ tính toán Compute trên AWS.
- Thực hành khởi tạo, cấu hình và kết nối Amazon EC2.
- Tìm hiểu các loại lưu trữ gắn với máy chủ như Amazon EBS và Amazon EFS.
- Nghiên cứu cơ chế tự động mở rộng hệ thống bằng Auto Scaling.
- Làm quen với Amazon CloudWatch để giám sát tài nguyên và thiết lập cảnh báo.
- Đánh giá sự khác nhau giữa kiến trúc sử dụng máy chủ EC2 và kiến trúc Serverless dự kiến áp dụng cho Smart Attendance SaaS.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Tìm hiểu Amazon EC2, các loại Instance, Amazon Machine Image, Key Pair, Security Group và quy trình khởi tạo một máy chủ ảo trên AWS. | 27/04/2026 | 27/04/2026 | Amazon EC2 Documentation |
| Thứ Ba | Thực hành tạo EC2 Instance, kết nối bằng SSH, cài đặt Web Server và kiểm tra khả năng truy cập ứng dụng từ Internet. | 28/04/2026 | 28/04/2026 | AWS EC2 Workshop |
| Thứ Tư | Tìm hiểu Amazon EBS và Amazon EFS; thực hành tạo Volume, gắn Volume vào EC2 và kiểm tra khả năng lưu trữ dữ liệu. | 29/04/2026 | 29/04/2026 | Amazon EBS và EFS Documentation |
| Thứ Năm | Nghiên cứu Launch Template, Auto Scaling Group và Elastic Load Balancing; tìm hiểu cơ chế tự động tăng hoặc giảm số lượng máy chủ theo tải hệ thống. | 30/04/2026 | 30/04/2026 | AWS Auto Scaling Documentation |
| Thứ Sáu | Sử dụng Amazon CloudWatch để theo dõi CPU, Network và trạng thái EC2; tạo CloudWatch Alarm; so sánh EC2 với AWS Lambda trong kiến trúc Smart Attendance SaaS. | 01/05/2026 | 01/05/2026 | Amazon CloudWatch Documentation |

### Kết quả thu hoạch thực tế sau Tuần 2:

- Hiểu được vai trò của Amazon EC2 trong việc cung cấp tài nguyên máy chủ ảo trên AWS.
- Khởi tạo và kết nối thành công EC2 Instance bằng SSH.
- Cài đặt thử nghiệm Web Server và kiểm tra ứng dụng hoạt động trên môi trường Cloud.
- Phân biệt được đặc điểm của Amazon EBS và Amazon EFS.
- Hiểu nguyên lý hoạt động của Launch Template, Elastic Load Balancing và Auto Scaling Group.
- Biết cách theo dõi các chỉ số cơ bản của máy chủ thông qua Amazon CloudWatch.
- Thiết lập được cảnh báo khi chỉ số CPU vượt quá ngưỡng cấu hình.
- Nhận thấy kiến trúc EC2 phù hợp với ứng dụng cần máy chủ hoạt động liên tục, trong khi kiến trúc Serverless phù hợp hơn với Smart Attendance SaaS ở giai đoạn MVP nhờ khả năng tự động mở rộng và tối ưu chi phí.