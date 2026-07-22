---
title: "Worklog Tuần 3"
date: 2026-05-04
weight: 3
chapter: false
pre: "<b>1.3. </b>"
---

# Worklog Tuần 3

### Mục tiêu định hướng trong tuần 3:

- Tìm hiểu chuyên sâu về hệ thống mạng trên AWS.
- Nắm được cấu trúc và vai trò của Amazon Virtual Private Cloud.
- Thực hành thiết kế Public Subnet, Private Subnet, Route Table, Internet Gateway và NAT Gateway.
- Phân biệt Security Group với Network Access Control List.
- Tìm hiểu Amazon Route 53, Elastic Load Balancing và các phương thức kết nối giữa nhiều mạng VPC.
- Vận dụng kiến thức mạng để chuẩn bị thiết kế hạ tầng cho Smart Attendance SaaS.

### Nhiệm vụ cụ thể cần triển khai:

| Ngày | Nhiệm vụ chi tiết | Ngày bắt đầu | Ngày hoàn thành | Nguồn tham khảo |
|---|---|---:|---:|---|
| Thứ Hai | Tìm hiểu Amazon VPC, dải địa chỉ CIDR, Public Subnet, Private Subnet và nguyên tắc phân chia mạng theo từng Availability Zone. | 04/05/2026 | 04/05/2026 | Amazon VPC Documentation |
| Thứ Ba | Thực hành tạo VPC, Subnet, Route Table và Internet Gateway; cấu hình đường đi để EC2 trong Public Subnet có thể kết nối Internet. | 05/05/2026 | 05/05/2026 | AWS Networking Workshop |
| Thứ Tư | Tìm hiểu NAT Gateway, Security Group và Network ACL; so sánh cơ chế Stateful và Stateless trong quá trình kiểm soát lưu lượng mạng. | 06/05/2026 | 06/05/2026 | AWS VPC Security Documentation |
| Thứ Năm | Nghiên cứu Elastic Load Balancing, Amazon Route 53, VPC Peering và Transit Gateway; tìm hiểu cách phân phối lưu lượng và liên kết các hệ thống mạng độc lập. | 07/05/2026 | 07/05/2026 | Route 53 và VPC Peering Documentation |
| Thứ Sáu | Tìm hiểu VPC Endpoint và AWS Systems Manager Session Manager; phác thảo luồng mạng dự kiến cho Smart Attendance SaaS; tổng kết kiến thức trong tuần. | 08/05/2026 | 08/05/2026 | AWS Systems Manager Documentation |

### Kết quả thu hoạch thực tế sau Tuần 3:

- Hiểu được vai trò của Amazon VPC trong việc xây dựng một môi trường mạng riêng biệt trên AWS.
- Biết cách lựa chọn dải CIDR và phân chia Public Subnet, Private Subnet theo từng Availability Zone.
- Tạo và cấu hình thành công VPC, Subnet, Route Table và Internet Gateway.
- Hiểu cách NAT Gateway hỗ trợ tài nguyên trong Private Subnet kết nối ra Internet mà không cho phép truy cập trực tiếp từ bên ngoài.
- Phân biệt được Security Group và Network ACL về phạm vi áp dụng, trạng thái kết nối và cơ chế thiết lập luật.
- Hiểu vai trò của Route 53 trong quản lý tên miền và định tuyến người dùng đến ứng dụng.
- Nắm được kiến thức cơ bản về VPC Peering, Transit Gateway và VPC Endpoint.
- Hoàn thành bản phác thảo luồng kết nối giữa người dùng, Frontend, API Gateway, Lambda và DynamoDB cho Smart Attendance SaaS.
- Củng cố kiến thức phù hợp với vai trò Cloud Infrastructure và Cloud Networking trong nhóm dự án.