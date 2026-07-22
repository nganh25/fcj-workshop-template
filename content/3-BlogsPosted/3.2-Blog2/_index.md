---
title: "AWS Backup – Item-Level Recovery cho Amazon EBS và Amazon S3"
date: 2026-07-21
weight: 2
chapter: false
pre: "<b>3.2. </b>"
---

# AWS Backup – Item-Level Recovery cho Amazon EBS và Amazon S3

Xin chào mọi người!

Một trong những tình huống phổ biến khi vận hành hệ thống là người dùng hoặc ứng dụng vô tình xóa nhầm dữ liệu. Đôi khi sự cố chỉ liên quan đến một file cấu hình, một tài liệu PDF, một file CSV hoặc một object nhỏ. Tuy nhiên, nếu chỉ có EBS Snapshot hoặc bản sao lưu toàn phần, đội ngũ vận hành có thể phải khôi phục cả volume chỉ để lấy lại một file.

AWS Backup Search và Item-Level Recovery giải quyết bài toán đó bằng cách cho phép tìm kiếm metadata và khôi phục chính xác file hoặc object cần thiết trong các bản sao lưu được AWS Backup quản lý cho Amazon EBS và Amazon S3.

## 1. Hạn chế của phương pháp khôi phục truyền thống

Trước đây, để lấy lại một file từ EBS Snapshot, quản trị viên thường phải:

1. Xác định snapshot phù hợp.
2. Khôi phục snapshot thành một EBS volume mới.
3. Gắn volume vào EC2 Instance.
4. Mount file system.
5. Tìm file cần khôi phục.
6. Sao chép file về hệ thống ban đầu.
7. Tháo và xóa tài nguyên tạm.

Quy trình này có thể làm tăng Recovery Time Objective (RTO), phát sinh chi phí lưu trữ tạm thời và yêu cầu nhiều thao tác thủ công.

Đối với Amazon S3, việc tìm đúng object trong nhiều recovery point cũng có thể mất thời gian nếu không có chỉ mục và tiêu chí tìm kiếm phù hợp.

## 2. Item-Level Recovery là gì?

Item-Level Recovery là khả năng khôi phục ở cấp độ từng file, folder hoặc object thay vì phải khôi phục toàn bộ bản sao lưu.

AWS Backup hỗ trợ tìm kiếm metadata trong:

- Amazon EBS snapshots được AWS Backup quản lý.
- Amazon S3 backups được AWS Backup quản lý.

Sau khi tìm thấy dữ liệu cần thiết, quản trị viên có thể khởi tạo restore job cho đúng item đã chọn.

Một số tình huống sử dụng thực tế:

- Khôi phục file cấu hình bị xóa nhầm.
- Lấy lại báo cáo CSV hoặc Excel cũ.
- Phục hồi tài liệu phục vụ kiểm toán.
- Tìm video, log hoặc file ứng dụng theo đường dẫn.
- Khôi phục một nhóm nhỏ object sau lỗi thao tác.
- Rút ngắn thời gian điều tra và phục hồi sau sự cố.

## 3. Backup Indexing hoạt động như thế nào?

Để tìm kiếm được dữ liệu, recovery point phải có **Backup Index**.

Backup Index lưu và lập danh mục metadata của file hoặc object. AWS Backup không lập chỉ mục nội dung bên trong file, nhờ đó hạn chế việc xử lý trực tiếp nội dung dữ liệu.

Các tiêu chí có thể dùng để tìm kiếm gồm:

### Với Amazon EBS

- File path.
- Creation time.
- Last modification time.
- File size.

### Với Amazon S3

- Object key.
- Object size.
- Creation time.
- ETag.
- Version ID.

Có thể bật indexing trong Backup Plan để mỗi backup mới tự động có index. Với recovery point hiện có, quản trị viên có thể tạo index thủ công từ AWS Backup Console hoặc AWS CLI.

## 4. Quy trình triển khai

### Bước 1: Chuẩn bị Backup Vault và Backup Plan

Tạo hoặc sử dụng Backup Vault hiện có, sau đó xây dựng Backup Plan cho EBS hoặc S3.

Nên xác định rõ:

- Tần suất backup.
- Thời gian lưu giữ.
- Chính sách chuyển sang cold storage.
- Cross-Region hoặc cross-account copy.
- Tag dùng để chọn tài nguyên.
- Yêu cầu tuân thủ và RPO.

### Bước 2: Bật Backup Index

Khi tạo On-Demand Backup hoặc Backup Rule, bật tùy chọn tạo Backup Index.

Đối với recovery point cũ:

1. Mở AWS Backup Console.
2. Chọn **Backup vaults**.
3. Mở vault chứa recovery point.
4. Chọn recovery point phù hợp.
5. Chọn **Create index**.

Lưu ý: recovery point cần nằm trong standard backup vault. Backup index hiện không được hỗ trợ cho recovery point trong logically air-gapped vault.

### Bước 3: Cấu hình IAM

Đối với EBS indexing, IAM role cần các quyền như:

```text
ec2:DescribeSnapshots
ebs:ListSnapshotBlocks
ebs:GetSnapshotBlock
kms:Decrypt
```

AWS cung cấp managed policy:

```text
AWSBackupServiceRolePolicyForIndexing
```

Đối với thao tác tìm kiếm, có thể sử dụng managed policy:

```text
AWSBackupSearchOperatorAccess
```

Đối với file-level restore từ EBS về Amazon S3, IAM role cần policy:

```text
AWSBackupServiceRolePolicyForItemRestores
```

Cần kiểm tra thêm KMS key policy nếu snapshot hoặc destination bucket dùng customer managed key.

### Bước 4: Tạo Search Job

Từ mục **Search** trong AWS Backup Console:

1. Chọn resource type là EBS hoặc S3.
2. Chọn các recovery point cần tìm.
3. Nhập tiêu chí tìm kiếm.
4. Chạy search job.
5. Xem và lọc kết quả.

Ví dụ:

- Tìm file có đường dẫn chứa `config`.
- Tìm object key kết thúc bằng `.csv`.
- Tìm file được tạo trong một khoảng thời gian.
- Tìm object lớn hơn một kích thước nhất định.

### Bước 5: Khôi phục item

#### Khôi phục file từ EBS Snapshot

File được chọn sẽ được khôi phục về một Amazon S3 bucket, không ghi đè trực tiếp lên file gốc trong EBS volume.

Trên AWS Backup Console, có thể nhập tối đa năm path trong một lần file-level restore. AWS CLI cho phép chỉ định nhiều path theo tham số của restore job.

Sau khi file xuất hiện trong S3, quản trị viên có thể:

1. Xác minh tính toàn vẹn.
2. Quét malware nếu cần.
3. Tải file xuống.
4. Chép file trở lại EBS volume hoặc ứng dụng đích.

#### Khôi phục object từ S3 Backup

Object có thể được khôi phục về:

- Bucket nguồn.
- Một bucket khác.

Nếu khôi phục về bucket nguồn, object được phục hồi tồn tại cùng với object hiện tại theo cơ chế và tùy chọn restore đã cấu hình, thay vì âm thầm ghi đè dữ liệu mà không kiểm soát.

## 5. Mã hóa dữ liệu khôi phục

Đối với EBS file-level restore về S3, có thể chọn:

- Default encryption của destination bucket.
- SSE-S3.
- SSE-KMS.

Nếu chọn SSE-KMS, cần bảo đảm IAM role và KMS key policy cho phép sử dụng khóa.

Đây là bước quan trọng khi môi trường Production có yêu cầu phân tách dữ liệu hoặc tuân thủ bảo mật.

## 6. Lợi ích đối với DevOps và Operations

### Rút ngắn RTO

Thay vì khôi phục cả volume, mount và tìm kiếm thủ công, đội ngũ vận hành có thể tìm đúng file dựa trên metadata và khôi phục trực tiếp.

### Giảm tài nguyên tạm

Không cần tạo một EBS volume đầy đủ chỉ để lấy lại vài file nhỏ trong nhiều trường hợp.

### Hỗ trợ điều tra sự cố

Có thể tìm file theo đường dẫn, thời gian hoặc kích thước để phục vụ audit, forensic và xử lý incident.

### Quản trị tập trung

AWS Backup cung cấp một nơi tập trung để quản lý backup, index, search và restore job.

### Tự động hóa tốt hơn

Backup index có thể được bật trong Backup Plan, giúp các recovery point mới sẵn sàng cho hoạt động tìm kiếm khi sự cố xảy ra.

## 7. Chi phí và giới hạn cần lưu ý

AWS tính phí dựa trên các thành phần như:

- Số item được lập chỉ mục.
- Dung lượng index.
- Số item được tìm kiếm.
- Số item được khôi phục.
- Dung lượng backup và destination storage.

Vì vậy, không nên bật indexing cho mọi backup một cách máy móc. Cần ưu tiên những workload có yêu cầu khôi phục file nhanh hoặc có giá trị dữ liệu cao.

Ngoài ra:

- EBS snapshot ở archive tier không hỗ trợ file-level restore trực tiếp.
- Cần theo dõi Region và feature availability.
- Phải kiểm tra file system, phân vùng và quyền IAM.
- Restore job cần được kiểm thử định kỳ, không chỉ cấu hình rồi để đó.
- Search index không thay thế chính sách backup và disaster recovery tổng thể.

## 8. Checklist triển khai thực tế

- [ ] Xác định workload cần item-level recovery.
- [ ] Thiết lập Backup Plan và retention phù hợp.
- [ ] Bật Backup Index cho recovery point quan trọng.
- [ ] Gắn tag nhất quán cho EC2, EBS, S3 và Backup Vault.
- [ ] Thêm IAM policy cho indexing, search và restore.
- [ ] Kiểm tra KMS key policy.
- [ ] Thực hiện thử một search job.
- [ ] Thực hiện thử file-level restore về S3.
- [ ] Xác minh dữ liệu sau restore.
- [ ] Theo dõi chi phí index và search.
- [ ] Tạo runbook xử lý sự cố.
- [ ] Kiểm thử restore định kỳ.

## 9. Kết hợp với chiến lược chống ransomware

Item-Level Recovery nên được kết hợp với:

- AWS Backup Vault Lock.
- Cross-account backup.
- Cross-Region copy.
- Least privilege IAM.
- AWS CloudTrail.
- Amazon GuardDuty Malware Protection for AWS Backup.
- Restore testing.
- Quy trình phê duyệt khi xóa recovery point.

Mục tiêu không chỉ là có backup mà còn phải bảo đảm backup không bị chỉnh sửa ngoài ý muốn và có thể khôi phục được trong thời gian yêu cầu.

## Kết luận

AWS Backup Search và Item-Level Recovery giúp thay đổi cách đội ngũ DevOps xử lý các tình huống mất file nhỏ. Thay vì khôi phục toàn bộ EBS volume hoặc dò tìm thủ công trong nhiều bản sao lưu, quản trị viên có thể sử dụng backup index để tìm đúng file hoặc object và khôi phục có chọn lọc.

Tính năng này có thể giúp rút ngắn RTO, giảm công việc thủ công và giảm tài nguyên tạm. Tuy nhiên, để sử dụng hiệu quả, doanh nghiệp cần chuẩn bị Backup Plan, IAM role, KMS permissions, indexing strategy và quy trình restore testing phù hợp.

## Nguồn tham khảo

- [AWS Storage Blog – Streamline search and item-level recovery with AWS Backup](https://aws.amazon.com/blogs/storage/streamline-search-and-item-level-recovery-with-aws-backup/)
- [AWS Backup Developer Guide – Backup search](https://docs.aws.amazon.com/aws-backup/latest/devguide/backup-search.html)
- [AWS Backup Developer Guide – Restore an Amazon EBS volume](https://docs.aws.amazon.com/aws-backup/latest/devguide/restoring-ebs.html)
- [AWS Storage Blog – Enable item-level search and recovery for Amazon EC2](https://aws.amazon.com/blogs/storage/enable-item-level-search-and-recovery-for-amazon-ec2-with-aws-backup/)

#AWS #AWSBackup #AmazonEBS #AmazonS3 #DisasterRecovery #DataProtection #DevOps #CloudComputing
