---
title: "Workshop"
date: 2026-07-22
weight: 5
chapter: false
pre: "<b>5. </b>"
---

# Xây dựng và triển khai Smart Attendance SaaS trên AWS

## Tổng quan

Workshop này được xây dựng trực tiếp từ cấu trúc mã nguồn của dự án **Smart Attendance SaaS**. Hệ thống sử dụng React cho Frontend và kiến trúc AWS Serverless cho Backend, bao gồm Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon DynamoDB, Amazon S3, Amazon CloudFront và các dịch vụ hỗ trợ giám sát, bảo mật, xử lý sự kiện.

Người tham gia sẽ đi từ bước chuẩn bị mã nguồn, chạy thử ứng dụng ở môi trường local, kiểm tra AWS SAM Template, triển khai Backend, kết nối Frontend, kiểm thử các chức năng điểm danh và cuối cùng dọn dẹp tài nguyên.

{{< staticimg src="images/5-Workshop/smart-attendance-architecture.png" alt="Kiến trúc Smart Attendance SaaS trên AWS" width="100%" >}}

## Mục tiêu

Sau khi hoàn thành workshop, bạn có thể:

- Hiểu cấu trúc Frontend và Backend của Smart Attendance SaaS.
- Chạy Backend Node.js và Frontend React ở môi trường local.
- Hiểu vai trò của `backend/template.yaml`.
- Triển khai API Gateway, Lambda, DynamoDB và Cognito bằng AWS SAM.
- Tích hợp JWT vào các request từ Frontend.
- Kiểm thử Clock-in, Clock-out, Attendance, Profile và Report.
- Triển khai React SPA lên Amazon S3 và Amazon CloudFront.
- Theo dõi log bằng Amazon CloudWatch.
- Rà soát các lớp bảo mật và xóa tài nguyên sau khi thực hành.

## Nội dung workshop

1. [Tổng quan kiến trúc và mã nguồn](5.1-workshop-overview/)
2. [Chuẩn bị môi trường](5.2-prerequisite/)
3. [Chạy thử và triển khai Backend](5.3-s3-vpc/)
4. [Tích hợp Frontend, Cognito và API](5.4-s3-onprem/)
5. [Kiểm thử, bảo mật và giám sát](5.5-policy/)
6. [Dọn dẹp tài nguyên](5.6-cleanup/)

## Thời lượng dự kiến

Khoảng **2 đến 3 giờ**, chưa bao gồm thời gian CloudFront hoàn tất triển khai.
