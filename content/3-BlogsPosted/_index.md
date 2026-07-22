---
title: "Các bài blogs đã đăng"
date: 2026-07-22
weight: 3
chapter: false
pre: "<b>3. </b>"
---

# Các bài blogs đã đăng

Trong quá trình tham gia chương trình **Workforce Bootcamp – First Cloud AI Journey**, tôi đã tìm hiểu và thực hiện ba bài viết liên quan đến bảo mật dữ liệu, sao lưu, khôi phục và tối ưu chi phí trên nền tảng Amazon Web Services.

Các bài viết tập trung vào những thay đổi và tính năng thực tế dành cho Developer, DevOps Engineer và Cloud Operations.

---

## [Blog 1 – AWS Security & S3: Amazon S3 đã vô hiệu hóa mặc định SSE-C từ tháng 04/2026](3.1-blog1/)

Bài viết trình bày thay đổi bảo mật mới của Amazon S3 đối với cơ chế **Server-Side Encryption with Customer-Provided Keys – SSE-C**.

Nội dung chính bao gồm:

- Phạm vi áp dụng đối với General Purpose Bucket mới.
- Ảnh hưởng đến các AWS Account chưa từng sử dụng SSE-C.
- Nguyên nhân request có thể nhận lỗi `HTTP 403 AccessDenied`.
- Cách rà soát source code, AWS SDK và pipeline.
- Cách bật lại SSE-C nếu hệ thống bắt buộc phải sử dụng.
- So sánh SSE-C với SSE-S3 và SSE-KMS.
- Cập nhật cấu hình Infrastructure as Code.

---

## [Blog 2 – AWS Backup: Item-Level Recovery cho Amazon EBS và Amazon S3](3.2-blog2/)

Bài viết giới thiệu **AWS Backup Search và Item-Level Recovery**, cho phép tìm kiếm và khôi phục từng file hoặc object riêng lẻ thay vì phải phục hồi toàn bộ bản sao lưu.

Nội dung chính bao gồm:

- Hạn chế của phương pháp khôi phục toàn bộ EBS Volume.
- Cơ chế Backup Indexing.
- Tìm kiếm file hoặc object dựa trên metadata.
- Khôi phục file từ Amazon EBS Snapshot.
- Khôi phục object từ Amazon S3 Backup.
- Cấu hình IAM Role và AWS KMS.
- Tối ưu Recovery Time Objective – RTO.
- Kết hợp AWS Backup Vault Lock để bảo vệ dữ liệu.

---

## [Blog 3 – AWS Cost Optimization: Vì sao nên chuyển Amazon EBS từ gp2 sang gp3?](3.3-blog3/)

Bài viết phân tích sự khác nhau giữa hai loại Amazon EBS General Purpose SSD là **gp2** và **gp3**.

Nội dung chính bao gồm:

- Cơ chế phân bổ IOPS của gp2.
- Khả năng tách biệt dung lượng, IOPS và Throughput của gp3.
- Baseline mặc định của gp3.
- Khả năng tiết kiệm tới 20% chi phí lưu trữ.
- Quy trình rà soát các volume gp2.
- Chuyển đổi bằng Amazon EBS Elastic Volumes.
- Cập nhật Terraform, CloudFormation hoặc AWS CDK.
- Theo dõi hiệu năng sau chuyển đổi bằng Amazon CloudWatch.
- Các lưu ý khi thực hiện chuyển đổi trong môi trường Production.

---

## Kết quả đạt được

Thông qua quá trình nghiên cứu và viết ba bài blog, tôi đã củng cố kiến thức về:

- Bảo mật và mã hóa dữ liệu trên Amazon S3.
- Quản lý khóa bằng AWS Key Management Service.
- Sao lưu và khôi phục dữ liệu với AWS Backup.
- Item-Level Recovery cho Amazon EBS và Amazon S3.
- Disaster Recovery và bảo vệ dữ liệu.
- Tối ưu chi phí Amazon EBS.
- Theo dõi hiệu năng bằng Amazon CloudWatch.
- Infrastructure as Code và tự động hóa vận hành.
- Các kỹ năng cần thiết cho Developer, DevOps và Cloud Engineer.