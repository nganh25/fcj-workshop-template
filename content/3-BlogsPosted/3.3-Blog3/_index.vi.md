---
title: "AWS Cost Optimization – Vì sao nên chuyển Amazon EBS từ gp2 sang gp3?"
date: 2026-07-22
weight: 3
chapter: false
pre: "<b>3.3. </b>"
---

# AWS Cost Optimization – Vì sao nên chuyển Amazon EBS từ gp2 sang gp3?

Xin chào mọi người!

Amazon Elastic Block Store (Amazon EBS) là dịch vụ block storage thường được sử dụng cùng Amazon EC2. Trong nhiều hệ thống đã vận hành lâu năm, EBS volume loại `gp2` vẫn còn xuất hiện với số lượng lớn vì đây từng là lựa chọn phổ biến cho General Purpose SSD.

`gp3` là thế hệ General Purpose SSD mới hơn. Điểm khác biệt quan trọng nhất là gp3 cho phép cấu hình dung lượng, IOPS và throughput tương đối độc lập. Nhờ đó, doanh nghiệp có thể cung cấp đúng mức hiệu năng cần thiết mà không phải tăng dung lượng chỉ để nhận thêm IOPS như với gp2.

AWS cho biết giá lưu trữ theo GiB của gp3 thấp hơn tới 20% so với gp2. Vì vậy, chuyển gp2 sang gp3 thường là một trong những quick win dễ triển khai trong chương trình FinOps.

## 1. gp2 và gp3 khác nhau như thế nào?

### gp2: Hiệu năng gắn với dung lượng

Với gp2, baseline performance được tính theo tỷ lệ khoảng 3 IOPS cho mỗi GiB, với giới hạn tối đa 16.000 IOPS.

Ví dụ, nếu workload cần baseline 3.000 IOPS, volume gp2 thường phải có dung lượng khoảng 1.000 GiB. Trong trường hợp dữ liệu thực tế chỉ sử dụng 200 GiB, doanh nghiệp vẫn phải trả tiền cho phần dung lượng lớn hơn để đạt mức IOPS mong muốn.

Các volume gp2 nhỏ có thể sử dụng burst credits. Điều này làm hiệu năng phụ thuộc vào kích thước volume và lượng credit còn lại.

### gp3: Tách dung lượng khỏi hiệu năng

gp3 cung cấp baseline gồm:

```text
3.000 IOPS
125 MiB/s throughput
```

Mức baseline này được bao gồm trong giá lưu trữ và không phụ thuộc vào việc volume chỉ có vài chục GiB hay lớn hơn.

Theo tài liệu Amazon EBS hiện tại, gp3 có thể được cấu hình đến:

```text
80.000 IOPS
2.000 MiB/s throughput
64 TiB dung lượng
```

Việc đạt giới hạn tối đa còn phụ thuộc vào kích thước volume, tỷ lệ IOPS/GiB, tỷ lệ throughput/IOPS và khả năng của EC2 Instance. Trên AWS Outposts, giới hạn gp3 thấp hơn.

## 2. Bảng so sánh nhanh

| Tiêu chí | gp2 | gp3 |
|---|---|---|
| Mô hình hiệu năng | Phụ thuộc dung lượng | IOPS và throughput cấu hình độc lập hơn |
| Baseline IOPS | Khoảng 3 IOPS/GiB | 3.000 IOPS |
| Baseline throughput | Phụ thuộc dung lượng | 125 MiB/s |
| Burst credits | Có với một số kích thước | Không sử dụng cơ chế burst |
| IOPS tối đa | 16.000 IOPS | Tối đa 80.000 IOPS theo điều kiện |
| Throughput tối đa | Tối đa 250 MiB/s | Tối đa 2.000 MiB/s theo điều kiện |
| Giá lưu trữ theo GiB | Cao hơn | Thấp hơn tới 20% |
| Mở rộng hiệu năng | Thường phải tăng dung lượng | Có thể mua thêm IOPS và throughput |

## 3. Lợi ích của việc chuyển sang gp3

### 3.1. Giảm chi phí lưu trữ

Với cùng dung lượng, gp3 có giá theo GiB thấp hơn tới 20% so với gp2. Mức tiết kiệm thực tế phụ thuộc vào Region, IOPS và throughput mua thêm.

Một volume gp3 yêu cầu hiệu năng cao hơn baseline có thể phát sinh thêm phí provisioned IOPS hoặc throughput. Vì vậy, cần tính toán tổng chi phí thay vì chỉ so sánh giá mỗi GiB.

### 3.2. Hiệu năng dễ dự đoán hơn

gp3 không dùng cơ chế burst credits. Volume có thể duy trì mức IOPS và throughput đã provision, miễn là EC2 Instance và workload không chạm giới hạn khác.

Điều này giúp đội ngũ vận hành giảm rủi ro hiệu năng thay đổi do cạn burst balance.

### 3.3. Không cần mua thêm dung lượng chỉ để tăng IOPS

Đây là lợi ích lớn nhất đối với database, API server, monitoring server và CI/CD worker. Có thể giữ dung lượng phù hợp với dữ liệu thực tế, sau đó cấu hình riêng mức IOPS hoặc throughput cần thiết.

### 3.4. Chuyển đổi bằng Elastic Volumes

Amazon EBS Elastic Volumes cho phép thay đổi volume type, IOPS và throughput của volume hiện có.

Đối với phần lớn EC2 Instance hiện đại và cấu hình được hỗ trợ, thao tác này có thể thực hiện mà không cần ngắt hoạt động của instance. Tuy nhiên, với một số instance thế hệ cũ hoặc trường hợp không hỗ trợ đầy đủ, AWS có thể yêu cầu detach non-root volume hoặc stop instance đối với root volume.

Vì vậy, không nên tuyên bố mọi trường hợp đều tuyệt đối zero-downtime. Đội ngũ DevOps cần kiểm tra yêu cầu của instance type và chuẩn bị rollback plan.

## 4. Workload nên được ưu tiên đánh giá

Các workload thường phù hợp để chuyển sang gp3:

- Web Application và API Server.
- Database nhỏ và trung bình.
- Bastion Host.
- Monitoring Server.
- Jenkins, GitLab Runner hoặc CI/CD Worker.
- File Server.
- Log processing node.
- Development và Test environment.
- EBS volume có dung lượng lớn nhưng IOPS sử dụng thấp.
- gp2 volume thường xuyên cạn burst balance.

Đối với workload yêu cầu độ trễ cực thấp hoặc IOPS rất cao và ổn định, cần đánh giá `io2 Block Express` thay vì mặc định chọn gp3.

## 5. Quy trình chuyển đổi an toàn

### Bước 1: Lập danh sách gp2 volume

Dùng AWS CLI:

```bash
aws ec2 describe-volumes \
  --filters Name=volume-type,Values=gp2 \
  --query "Volumes[].{VolumeId:VolumeId,Size:Size,Iops:Iops,State:State,AZ:AvailabilityZone}" \
  --output table
```

Có thể kết hợp:

- AWS Cost Optimization Hub.
- AWS Compute Optimizer.
- AWS Cost Explorer.
- AWS Config.
- Resource Explorer.
- Tagging strategy.

### Bước 2: Thu thập hiệu năng thực tế

Theo dõi ít nhất một chu kỳ tải đại diện. Với hệ thống có cao điểm theo tuần hoặc tháng, thời gian quan sát cần đủ dài để không bỏ sót spike.

Các chỉ số cần xem:

- `VolumeReadOps`
- `VolumeWriteOps`
- `VolumeReadBytes`
- `VolumeWriteBytes`
- `VolumeQueueLength`
- `VolumeAvgIOPS`
- `VolumeAvgThroughput`
- `VolumeAvgReadLatency`
- `VolumeAvgWriteLatency`
- `VolumeIOPSExceededCheck`
- `VolumeThroughputExceededCheck`

Một số metric nâng cao chỉ khả dụng với volume gắn vào Nitro-based Instance phù hợp.

Cũng cần kiểm tra giới hạn EBS của EC2 Instance thông qua:

- `InstanceEBSIOPSExceededCheck`
- `InstanceEBSThroughputExceededCheck`

Nếu instance đã chạm giới hạn EBS, tăng IOPS trên volume có thể không cải thiện hiệu năng.

### Bước 3: Tạo snapshot trước khi thay đổi

Trước khi chuyển đổi Production volume, nên tạo EBS Snapshot và xác nhận trạng thái hoàn tất.

```bash
aws ec2 create-snapshot \
  --volume-id vol-xxxxxxxxxxxxxxxxx \
  --description "Pre-migration snapshot gp2 to gp3"
```

Snapshot không thay thế kế hoạch backup tổng thể, nhưng cung cấp một recovery point trước khi thay đổi cấu hình.

### Bước 4: Chuyển gp2 sang gp3

Ví dụ chuyển volume sang gp3 với baseline:

```bash
aws ec2 modify-volume \
  --volume-id vol-xxxxxxxxxxxxxxxxx \
  --volume-type gp3 \
  --iops 3000 \
  --throughput 125
```

Theo dõi trạng thái:

```bash
aws ec2 describe-volumes-modifications \
  --volume-ids vol-xxxxxxxxxxxxxxxxx
```

Trạng thái thường đi qua các bước như:

```text
modifying
optimizing
completed
```

Khi chuyển gp2 sang gp3 mà không chỉ định IOPS và throughput, Amazon EBS có thể tự provision mức tương đương volume nguồn hoặc baseline gp3, chọn mức cao hơn. Tuy nhiên, trong môi trường Production nên xác định rõ giá trị mong muốn và kiểm tra chi phí trước khi chạy.

### Bước 5: Cập nhật Infrastructure as Code

Terraform:

```hcl
resource "aws_ebs_volume" "application_data" {
  availability_zone = "ap-southeast-1a"
  size              = 200
  type              = "gp3"
  iops              = 3000
  throughput        = 125

  tags = {
    Name = "application-data"
  }
}
```

CloudFormation:

```yaml
Resources:
  ApplicationVolume:
    Type: AWS::EC2::Volume
    Properties:
      AvailabilityZone: ap-southeast-1a
      Size: 200
      VolumeType: gp3
      Iops: 3000
      Throughput: 125
      Tags:
        - Key: Name
          Value: application-data
```

Việc cập nhật IaC giúp tránh tình trạng lần deploy sau đưa volume trở lại gp2 hoặc tạo volume mới với cấu hình cũ.

### Bước 6: Giám sát sau chuyển đổi

Theo dõi trong thời gian phù hợp với chu kỳ tải:

- IOPS thực tế.
- Throughput thực tế.
- Queue length.
- Read/write latency.
- Application response time.
- Database latency.
- Error rate.
- EBS exceeded checks.
- Chi phí EBS sau chuyển đổi.

Nếu thường xuyên chạm mức 3.000 IOPS hoặc 125 MiB/s, tăng riêng IOPS hoặc throughput thay vì tăng dung lượng.

## 6. Những sai lầm thường gặp

### Chuyển đổi hàng loạt mà không đo tải

Một số gp2 volume lớn có thể đang cung cấp throughput cao hơn baseline gp3. Nếu chuyển sang gp3 và ép về 125 MiB/s mà không kiểm tra, hiệu năng có thể giảm.

### Chỉ nhìn vào volume, bỏ qua instance

Mỗi EC2 Instance có giới hạn EBS bandwidth và IOPS riêng. Volume cấu hình 20.000 IOPS không có nghĩa instance luôn sử dụng được toàn bộ.

### Không cập nhật IaC

Thay đổi thủ công trên Console có thể tạo configuration drift và bị ghi đè trong lần deploy tiếp theo.

### Không kiểm tra chi phí IOPS và throughput bổ sung

gp3 rẻ hơn theo dung lượng, nhưng phần hiệu năng vượt baseline có tính phí. Cần tính tổng chi phí sau cùng.

### Không chuẩn bị snapshot và rollback

Dù Elastic Volumes được thiết kế để thay đổi trực tuyến, mọi thay đổi Production vẫn cần snapshot, change ticket, maintenance plan và tiêu chí rollback.

## 7. Checklist FinOps

- [ ] Liệt kê toàn bộ gp2 volume.
- [ ] Gắn tag Owner, Environment, Application và CostCenter.
- [ ] Thu thập CloudWatch Metrics.
- [ ] Xác định gp3 IOPS và throughput phù hợp.
- [ ] Ước tính chi phí trước và sau.
- [ ] Kiểm tra giới hạn EBS của EC2 Instance.
- [ ] Tạo snapshot trước khi chuyển đổi.
- [ ] Thử nghiệm trên Development hoặc Staging.
- [ ] Cập nhật Terraform, CloudFormation hoặc CDK.
- [ ] Theo dõi trạng thái volume modification.
- [ ] Giám sát ứng dụng sau chuyển đổi.
- [ ] Xác nhận số tiền tiết kiệm thực tế.

## Kết luận

Chuyển Amazon EBS từ gp2 sang gp3 thường là một hành động tối ưu chi phí có hiệu quả cao. gp3 cung cấp baseline 3.000 IOPS và 125 MiB/s, tách hiệu năng khỏi dung lượng tốt hơn và có giá lưu trữ theo GiB thấp hơn tới 20%.

Tuy nhiên, chuyển đổi không nên được thực hiện chỉ bằng một thao tác hàng loạt. Quy trình đúng cần bao gồm inventory, đo tải, tính chi phí, kiểm tra giới hạn EC2, tạo snapshot, cập nhật IaC và giám sát sau thay đổi.

Khi được thực hiện có kiểm soát, gp3 giúp doanh nghiệp giảm chi phí EBS, đơn giản hóa capacity planning và duy trì hiệu năng phù hợp hơn với nhu cầu thực tế.

## Nguồn tham khảo

- [AWS Storage Blog – Migrate EBS volumes from gp2 to gp3 and save up to 20%](https://aws.amazon.com/blogs/storage/migrate-your-amazon-ebs-volumes-from-gp2-to-gp3-and-save-up-to-20-on-costs/)
- [Amazon EBS User Guide – General Purpose SSD volumes](https://docs.aws.amazon.com/ebs/latest/userguide/general-purpose.html)
- [Amazon EBS User Guide – Modify a volume using Elastic Volumes](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-modify-volume.html)
- [Amazon EBS User Guide – CloudWatch metrics for EBS](https://docs.aws.amazon.com/ebs/latest/userguide/using_cloudwatch_ebs.html)

#AWS #AmazonEBS #AWSStorage #CostOptimization #FinOps #AmazonEC2 #CloudComputing #DevOps
