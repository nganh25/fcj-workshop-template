---
title: "AWS Security & S3 – Amazon S3 đã vô hiệu hóa mặc định SSE-C từ tháng 04/2026"
date: 2026-07-20
weight: 1
chapter: false
pre: "<b>3.1. </b>"
---

# AWS Security & S3 – Amazon S3 đã vô hiệu hóa mặc định SSE-C từ tháng 04/2026

Xin chào mọi người!

Amazon Simple Storage Service (Amazon S3) là một trong những dịch vụ lưu trữ được sử dụng phổ biến nhất trên AWS. S3 có thể phục vụ nhiều loại workload như website tĩnh, lưu trữ tài liệu, sao lưu dữ liệu, data lake, log hệ thống và dữ liệu cho các ứng dụng AI/ML.

Bên cạnh khả năng mở rộng và độ bền dữ liệu cao, bảo mật luôn là một phần quan trọng trong quá trình thiết kế hệ thống S3. Từ tháng 04/2026, AWS đã triển khai một thay đổi mặc định mới: **Server-Side Encryption with Customer-Provided Keys (SSE-C) bị chặn đối với các yêu cầu ghi mới trên toàn bộ General Purpose Bucket mới**. Một số bucket hiện có cũng được áp dụng thay đổi nếu tài khoản AWS chưa từng có object được mã hóa bằng SSE-C.

Thay đổi này không có nghĩa là AWS loại bỏ hoàn toàn SSE-C. Tuy nhiên, các hệ thống thực sự cần SSE-C phải chủ động bật lại tính năng này ở cấp bucket.

## 1. SSE-C là gì?

SSE-C là cơ chế mã hóa phía máy chủ trong đó khách hàng tự cung cấp khóa mã hóa cho Amazon S3 trong mỗi request. Amazon S3 sử dụng khóa đó để mã hóa hoặc giải mã object nhưng không lưu trữ khóa mã hóa do khách hàng cung cấp.

Khi upload object bằng SSE-C, ứng dụng thường gửi ba header chính:

```text
x-amz-server-side-encryption-customer-algorithm
x-amz-server-side-encryption-customer-key
x-amz-server-side-encryption-customer-key-MD5
```

Cơ chế này cho phép doanh nghiệp kiểm soát hoàn toàn khóa mã hóa. Đổi lại, toàn bộ trách nhiệm quản lý khóa thuộc về phía ứng dụng. Nếu khóa bị mất, bị thay đổi hoặc gửi sai, dữ liệu sẽ không thể được giải mã.

## 2. Những thay đổi quan trọng từ tháng 04/2026

### 2.1. Bucket mới chặn SSE-C theo mặc định

Mọi General Purpose Bucket mới được tạo sẽ chặn các yêu cầu ghi object sử dụng SSE-C theo mặc định. Ứng dụng muốn tiếp tục sử dụng SSE-C phải chủ động thay đổi cấu hình bucket sau khi tạo.

### 2.2. Một số bucket hiện có cũng được cập nhật

Nếu tài khoản AWS chưa từng có object được mã hóa bằng SSE-C, AWS cũng chặn SSE-C đối với các yêu cầu ghi mới trên những bucket hiện có của tài khoản đó.

Ngược lại, nếu tài khoản đã có ít nhất một object sử dụng SSE-C, AWS không tự thay đổi cấu hình của các bucket hiện có trong tài khoản. Tuy nhiên, các bucket mới vẫn áp dụng cấu hình chặn SSE-C theo mặc định.

### 2.3. Request ghi có thể nhận lỗi HTTP 403

Khi SSE-C đang bị chặn, những thao tác như `PutObject`, `CopyObject`, `PostObject`, multipart upload hoặc replication sử dụng SSE-C sẽ bị từ chối với lỗi:

```text
HTTP 403 AccessDenied
```

Điều này có thể gây gián đoạn cho pipeline upload, backup script, dịch vụ chia sẻ file hoặc ứng dụng cũ vẫn gửi SSE-C header.

### 2.4. Object SSE-C đã tồn tại không bị thay đổi

Việc chặn SSE-C chỉ ảnh hưởng đến các yêu cầu ghi mới. Amazon S3 không thay đổi phương thức mã hóa của những object SSE-C đã tồn tại.

Ứng dụng vẫn có thể thực hiện `GetObject` hoặc `HeadObject` đối với object cũ nếu cung cấp đúng các SSE-C header và đúng khóa mã hóa.

## 3. Vì sao AWS thực hiện thay đổi này?

SSE-C mang lại quyền kiểm soát khóa rất cao nhưng cũng làm tăng rủi ro vận hành:

- Ứng dụng phải tự lưu trữ và phân phối khóa.
- Khóa phải được gửi trong từng request đọc hoặc ghi.
- Mất khóa đồng nghĩa với mất khả năng giải mã dữ liệu.
- Việc luân chuyển, kiểm toán và phân quyền khóa phức tạp hơn.
- Cấu hình sai có thể gây lỗi cho toàn bộ pipeline dữ liệu.

Với phần lớn workload hiện đại, AWS khuyến nghị giữ SSE-C ở trạng thái bị chặn, trừ khi có yêu cầu kỹ thuật hoặc tuân thủ cụ thể. SSE-S3 và SSE-KMS thường đơn giản hơn trong quá trình vận hành và tích hợp tốt hơn với hệ sinh thái bảo mật AWS.

## 4. Developer và DevOps cần làm gì?

### Bước 1: Kiểm tra cấu hình bucket

Có thể dùng AWS CLI để xem cấu hình mã hóa và trạng thái chặn SSE-C:

```bash
aws s3api get-bucket-encryption \
  --bucket ten-bucket-cua-ban
```

Kết quả có thể chứa trường `BlockedEncryptionTypes`. Nếu giá trị là `SSE-C`, các yêu cầu ghi sử dụng SSE-C đang bị chặn.

IAM principal thực hiện kiểm tra cần quyền:

```text
s3:GetEncryptionConfiguration
```

### Bước 2: Rà soát mã nguồn và pipeline

Tìm trong source code, biến môi trường, file cấu hình CI/CD và script vận hành các từ khóa:

```text
SSECustomerAlgorithm
SSECustomerKey
SSECustomerKeyMD5
x-amz-server-side-encryption-customer-
```

Cần kiểm tra cả:

- AWS SDK cũ.
- Script AWS CLI.
- Công cụ đồng bộ dữ liệu.
- Backup agent.
- Pipeline upload file.
- Data ingestion pipeline.
- Ứng dụng bên thứ ba tích hợp với S3.

### Bước 3: Bật lại SSE-C nếu bắt buộc

Nếu hệ thống thực sự cần SSE-C, quản trị viên có thể cho phép lại bằng AWS CLI:

```bash
aws s3api put-bucket-encryption \
  --bucket ten-bucket-cua-ban \
  --server-side-encryption-configuration '{
    "Rules": [{
      "BlockedEncryptionTypes": {
        "EncryptionType": "NONE"
      }
    }]
  }'
```

IAM principal cần quyền:

```text
s3:PutEncryptionConfiguration
```

Để chặn SSE-C trở lại:

```bash
aws s3api put-bucket-encryption \
  --bucket ten-bucket-cua-ban \
  --server-side-encryption-configuration '{
    "Rules": [{
      "BlockedEncryptionTypes": {
        "EncryptionType": "SSE-C"
      }
    }]
  }'
```

### Bước 4: Cập nhật Infrastructure as Code

Không nên bật hoặc tắt SSE-C thủ công rồi để cấu hình IaC không đồng bộ. Cần cập nhật CloudFormation, AWS CDK hoặc Terraform để cấu hình được quản lý thống nhất.

Ví dụ AWS CloudFormation:

```yaml
Resources:
  ApplicationBucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketEncryption:
        ServerSideEncryptionConfiguration:
          - BlockedEncryptionTypes:
              EncryptionType:
                - SSE-C
```

Ví dụ AWS CDK với TypeScript:

```typescript
const bucket = new s3.Bucket(this, "ApplicationBucket", {
  blockedEncryptionTypes: [
    s3.BlockedEncryptionType.SSE_C,
  ],
});
```

Nếu ứng dụng bắt buộc phải cho phép SSE-C, sử dụng giá trị `NONE` theo tài liệu của công cụ IaC đang dùng và kiểm thử trên môi trường Development trước khi triển khai Production.

## 5. Nên chuyển sang SSE-S3 hay SSE-KMS?

### SSE-S3

SSE-S3 phù hợp khi cần giải pháp đơn giản, không phải tự quản lý khóa và không cần kiểm soát khóa chi tiết. Tất cả object mới trong S3 đều được mã hóa mặc định bằng SSE-S3 nếu không cấu hình phương thức khác.

Ưu điểm:

- Dễ sử dụng.
- Không cần quản lý AWS KMS key.
- Phù hợp cho website tĩnh, log thông thường, dữ liệu ứng dụng và backup không có yêu cầu quản lý khóa riêng.

### SSE-KMS

SSE-KMS phù hợp với môi trường Production cần kiểm soát và kiểm toán tốt hơn.

Ưu điểm:

- Phân quyền truy cập khóa bằng IAM và KMS key policy.
- Theo dõi hoạt động sử dụng khóa bằng AWS CloudTrail.
- Hỗ trợ customer managed key.
- Có thể bật automatic key rotation cho customer managed key.
- Phù hợp với các yêu cầu bảo mật và tuân thủ cao hơn.

Khi sử dụng SSE-KMS cho khối lượng request lớn, nên cân nhắc bật **S3 Bucket Keys** để giảm số lượng request trực tiếp đến AWS KMS và tối ưu chi phí.

## 6. Checklist triển khai an toàn

- [ ] Liệt kê tất cả S3 bucket trong từng tài khoản và Region.
- [ ] Kiểm tra `BlockedEncryptionTypes`.
- [ ] Tìm các SSE-C header trong source code và pipeline.
- [ ] Kiểm tra CloudTrail để xác định request đang dùng SSE-C.
- [ ] Xác nhận ứng dụng có thực sự cần SSE-C hay không.
- [ ] Chuyển sang SSE-S3 hoặc SSE-KMS nếu không có yêu cầu bắt buộc.
- [ ] Cập nhật CloudFormation, CDK hoặc Terraform.
- [ ] Kiểm thử upload, download, multipart upload và replication.
- [ ] Thiết lập cảnh báo đối với lỗi `403 AccessDenied`.
- [ ] Lưu kế hoạch rollback trước khi triển khai Production.

## Kết luận

Thay đổi mặc định của Amazon S3 từ tháng 04/2026 là một bước tăng cường bảo mật nhằm giảm nguy cơ cấu hình sai đối với người dùng mới. SSE-C vẫn còn được hỗ trợ, nhưng phải được bật lại có chủ đích trên bucket nếu workload thực sự cần sử dụng.

Đối với Developer và DevOps, việc quan trọng nhất là rà soát các request S3 hiện có, cập nhật Infrastructure as Code và lựa chọn phương thức mã hóa phù hợp. Trong phần lớn trường hợp, SSE-S3 hoặc SSE-KMS sẽ giúp hệ thống dễ vận hành, dễ kiểm toán và giảm rủi ro mất khóa hơn so với SSE-C.

## Nguồn tham khảo

- [Amazon S3 – Default SSE-C setting FAQ](https://docs.aws.amazon.com/AmazonS3/latest/userguide/default-s3-c-encryption-setting-faq.html)
- [Amazon S3 – Blocking or unblocking SSE-C](https://docs.aws.amazon.com/AmazonS3/latest/userguide/blocking-unblocking-s3-c-encryption-gpb.html)
- [AWS What's New – S3 default bucket security setting](https://aws.amazon.com/about-aws/whats-new/2026/04/s3-default-bucket-security-setting/)
- [AWS CloudFormation – BlockedEncryptionTypes](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-s3-bucket-blockedencryptiontypes.html)

#AWS #AmazonS3 #CloudSecurity #SSEKMS #SSEC #DevOps #CloudComputing #AWSStorage
