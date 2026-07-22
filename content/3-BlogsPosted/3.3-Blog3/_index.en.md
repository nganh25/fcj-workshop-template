---
title: "AWS Cost Optimization – Why Move Amazon EBS from gp2 to gp3?"
date: 2026-07-22
weight: 3
chapter: false
pre: "<b>3.3. </b>"
---

# AWS Cost Optimization – Why Move Amazon EBS from gp2 to gp3?

Hello everyone!

Amazon Elastic Block Store (Amazon EBS) provides block storage for Amazon EC2. Many long-running environments still contain a large number of `gp2` General Purpose SSD volumes because gp2 was the standard choice for years.

`gp3` is the newer General Purpose SSD generation. Its most important architectural difference is that storage capacity, IOPS, and throughput can be configured more independently. This allows an organization to provision the performance it needs without increasing storage capacity only to receive additional IOPS.

AWS states that gp3 storage pricing per GiB is up to 20% lower than gp2. As a result, moving suitable workloads from gp2 to gp3 is often a practical FinOps quick win.

## 1. How Do gp2 and gp3 Differ?

### gp2: Performance Is Linked to Capacity

The baseline performance of gp2 is approximately 3 IOPS per GiB, up to 16,000 IOPS.

For example, a workload that needs a baseline of 3,000 IOPS generally needs a gp2 volume of about 1,000 GiB. If the application only stores 200 GiB of data, the organization still pays for the larger volume to obtain the desired baseline performance.

Smaller gp2 volumes can use burst credits. Their performance behavior therefore depends on volume size and the available burst balance.

### gp3: Capacity and Performance Are More Independent

A gp3 volume includes the following baseline:

```text
3,000 IOPS
125 MiB/s throughput
```

This baseline is included in the storage price and does not depend on whether the volume stores tens of GiB or much more.

Current Amazon EBS documentation allows gp3 configurations up to:

```text
80,000 IOPS
2,000 MiB/s throughput
64 TiB capacity
```

Reaching the maximum depends on volume size, the IOPS-per-GiB ratio, the throughput-to-IOPS ratio, the EC2 instance, and Region or platform limitations. AWS Outposts has different limits.

## 2. Quick Comparison

| Area | gp2 | gp3 |
|---|---|---|
| Performance model | Linked to capacity | IOPS and throughput are more independently configurable |
| Baseline IOPS | About 3 IOPS/GiB | 3,000 IOPS |
| Baseline throughput | Depends on volume size | 125 MiB/s |
| Burst credits | Used by some sizes | No burst-credit model |
| Maximum IOPS | 16,000 | Up to 80,000 under supported conditions |
| Maximum throughput | Up to 250 MiB/s | Up to 2,000 MiB/s under supported conditions |
| Storage price per GiB | Higher | Up to 20% lower |
| Scaling performance | Often requires more GiB | Add IOPS or throughput separately |

## 3. Benefits of Moving to gp3

### 3.1. Lower Storage Cost

For the same capacity, gp3 storage is priced up to 20% lower than gp2. Actual savings depend on the Region and any additional provisioned IOPS or throughput.

A gp3 volume configured above the included baseline incurs extra performance charges. Always compare the total monthly cost rather than only the price per GiB.

### 3.2. More Predictable Performance

gp3 does not use burst credits. It can maintain the provisioned IOPS and throughput as long as the EC2 instance and the workload do not hit another limit.

This reduces performance variation caused by depleted gp2 burst credits.

### 3.3. No Need to Buy Capacity Only for IOPS

This is valuable for databases, API servers, monitoring systems, and CI/CD workers. Storage can remain aligned with actual data growth while IOPS and throughput are provisioned according to performance requirements.

### 3.4. Migration with Elastic Volumes

Amazon EBS Elastic Volumes allows operators to modify the volume type, IOPS, and throughput of an existing volume.

For most modern supported EC2 instances, the modification can be performed while the volume remains attached and the instance continues running. Older instance families or unsupported configurations can require a detach operation for a data volume or a stop operation for a root volume.

Therefore, do not assume that every migration is automatically zero-downtime. Review the instance and volume requirements and prepare a change and rollback plan.

## 4. Workloads to Evaluate First

Typical candidates include:

- Web applications and API servers.
- Small and medium databases.
- Bastion hosts.
- Monitoring servers.
- Jenkins, GitLab Runner, and other CI/CD workers.
- File servers.
- Log-processing nodes.
- Development and test environments.
- Large gp2 volumes with low actual IOPS usage.
- gp2 volumes that frequently consume their burst balance.

For workloads that need consistently ultra-low latency or performance beyond gp3 limits, evaluate `io2 Block Express` instead.

## 5. A Safe Migration Process

### Step 1: Inventory Existing gp2 Volumes

Use AWS CLI:

```bash
aws ec2 describe-volumes \
  --filters Name=volume-type,Values=gp2 \
  --query "Volumes[].{VolumeId:VolumeId,Size:Size,Iops:Iops,State:State,AZ:AvailabilityZone}" \
  --output table
```

Also consider:

- AWS Cost Optimization Hub.
- AWS Compute Optimizer.
- AWS Cost Explorer.
- AWS Config.
- AWS Resource Explorer.
- A consistent tagging strategy.

### Step 2: Collect Real Performance Data

Monitor at least one representative workload cycle. Systems with weekly or monthly peaks need a longer observation window.

Useful metrics include:

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

Some enhanced metrics require an eligible volume attached to a Nitro-based instance.

Also inspect instance-level EBS limits with:

- `InstanceEBSIOPSExceededCheck`
- `InstanceEBSThroughputExceededCheck`

If the EC2 instance is already at its EBS limit, increasing volume performance alone may not improve the application.

### Step 3: Create a Snapshot

Before changing a production volume, create an EBS snapshot and wait for it to complete:

```bash
aws ec2 create-snapshot \
  --volume-id vol-xxxxxxxxxxxxxxxxx \
  --description "Pre-migration snapshot gp2 to gp3"
```

A snapshot does not replace a full backup strategy, but it provides a recovery point before the configuration change.

### Step 4: Modify the Volume

Example migration to gp3 baseline performance:

```bash
aws ec2 modify-volume \
  --volume-id vol-xxxxxxxxxxxxxxxxx \
  --volume-type gp3 \
  --iops 3000 \
  --throughput 125
```

Track progress:

```bash
aws ec2 describe-volumes-modifications \
  --volume-ids vol-xxxxxxxxxxxxxxxxx
```

The modification generally moves through states such as:

```text
modifying
optimizing
completed
```

When changing gp2 to gp3 without specifying IOPS and throughput, Amazon EBS can derive values from the source performance and gp3 baseline. For production, it is safer to define the intended target explicitly and validate both performance and price.

### Step 5: Update Infrastructure as Code

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

Updating IaC prevents the next deployment from recreating gp2 volumes or reverting a manually changed configuration.

### Step 6: Monitor After Migration

Observe the volume for a period that covers normal and peak demand:

- Actual IOPS.
- Actual throughput.
- Queue length.
- Read and write latency.
- Application response time.
- Database latency.
- Error rate.
- EBS exceeded checks.
- EBS cost after migration.

If the workload repeatedly reaches 3,000 IOPS or 125 MiB/s, increase IOPS or throughput without increasing storage capacity.

## 6. Common Mistakes

### Migrating in Bulk Without Measuring Demand

Some large gp2 volumes may deliver higher throughput than the gp3 baseline. Moving them to gp3 with only 125 MiB/s without measuring usage can reduce performance.

### Looking Only at the Volume

Each EC2 instance has its own EBS bandwidth and IOPS limit. A volume configured for 20,000 IOPS does not guarantee that the instance can consume all of it.

### Failing to Update IaC

Manual Console changes create configuration drift and can be overwritten by a later deployment.

### Ignoring Additional IOPS and Throughput Charges

gp3 is cheaper per GiB, but performance above the included baseline is billed separately. Calculate total cost.

### Skipping Snapshots and Rollback Planning

Elastic Volumes is designed for online modification, but a production change still requires a recovery point, a maintenance plan, validation criteria, and a rollback decision process.

## 7. FinOps Checklist

- [ ] Inventory all gp2 volumes.
- [ ] Apply Owner, Environment, Application, and CostCenter tags.
- [ ] Collect CloudWatch metrics.
- [ ] Determine appropriate gp3 IOPS and throughput.
- [ ] Estimate cost before and after migration.
- [ ] Check EC2 instance EBS limits.
- [ ] Create a snapshot before migration.
- [ ] Test in Development or Staging.
- [ ] Update Terraform, CloudFormation, or CDK.
- [ ] Monitor the volume modification.
- [ ] Monitor the application after migration.
- [ ] Confirm the actual monthly savings.

## Conclusion

Moving Amazon EBS from gp2 to gp3 is often a high-value cost optimization. gp3 includes 3,000 IOPS and 125 MiB/s, provides more independent performance configuration, and has a storage price per GiB that is up to 20% lower than gp2.

However, the migration should not be treated as an unplanned bulk update. A safe process includes inventory, performance measurement, cost estimation, EC2 limit checks, snapshots, IaC updates, and post-change monitoring.

When implemented carefully, gp3 can reduce EBS cost, simplify capacity planning, and provide performance that is better aligned with actual workload requirements.

## References

- [AWS Storage Blog – Migrate EBS volumes from gp2 to gp3 and save up to 20%](https://aws.amazon.com/blogs/storage/migrate-your-amazon-ebs-volumes-from-gp2-to-gp3-and-save-up-to-20-on-costs/)
- [Amazon EBS User Guide – General Purpose SSD volumes](https://docs.aws.amazon.com/ebs/latest/userguide/general-purpose.html)
- [Amazon EBS User Guide – Modify a volume using Elastic Volumes](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-modify-volume.html)
- [Amazon EBS User Guide – CloudWatch metrics for EBS](https://docs.aws.amazon.com/ebs/latest/userguide/using_cloudwatch_ebs.html)

#AWS #AmazonEBS #AWSStorage #CostOptimization #FinOps #AmazonEC2 #CloudComputing #DevOps
