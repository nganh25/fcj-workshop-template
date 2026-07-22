---
title: "Published Blogs"
date: 2026-07-22
weight: 3
chapter: false
pre: "<b>3. </b>"
---

# Published Blogs

During the **Workforce Bootcamp – First Cloud AI Journey**, I researched and completed three articles about data security, backup and recovery, and cost optimization on Amazon Web Services.

The articles focus on practical changes and capabilities that are relevant to Developers, DevOps Engineers, and Cloud Operations teams.

---

## [Blog 1 – AWS Security & S3: Amazon S3 Has Disabled SSE-C by Default Since April 2026](3.1-blog1/)

This article explains Amazon S3's updated default security setting for **Server-Side Encryption with Customer-Provided Keys (SSE-C)**.

Main topics include:

- How the change applies to new general purpose buckets.
- The effect on AWS accounts that have never used SSE-C.
- Why write requests can receive `HTTP 403 AccessDenied`.
- How to review source code, SDK usage, and data pipelines.
- How to re-enable SSE-C when a workload has a specific requirement.
- The differences between SSE-C, SSE-S3, and SSE-KMS.
- How to keep Infrastructure as Code aligned with the intended configuration.

---

## [Blog 2 – AWS Backup: Item-Level Recovery for Amazon EBS and Amazon S3](3.2-blog2/)

This article introduces **AWS Backup Search and Item-Level Recovery**, which allow operators to locate and restore individual files or objects instead of recovering an entire backup.

Main topics include:

- Limitations of full-volume recovery for Amazon EBS.
- How Backup Indexing works.
- Searching for files or objects by metadata.
- Restoring files from Amazon EBS snapshots.
- Restoring objects from Amazon S3 backups.
- IAM role and AWS KMS requirements.
- Improving the Recovery Time Objective (RTO).
- Combining recovery capabilities with AWS Backup Vault Lock.

---

## [Blog 3 – AWS Cost Optimization: Why Move Amazon EBS from gp2 to gp3?](3.3-blog3/)

This article compares the `gp2` and `gp3` Amazon EBS General Purpose SSD volume types.

Main topics include:

- The capacity-linked IOPS model of gp2.
- Independent capacity, IOPS, and throughput configuration with gp3.
- The included gp3 baseline performance.
- Potential storage savings of up to 20%.
- How to inventory existing gp2 volumes.
- Migration with Amazon EBS Elastic Volumes.
- Updating Terraform, CloudFormation, or AWS CDK.
- Monitoring performance after migration with Amazon CloudWatch.
- Production migration and rollback considerations.

---

## Learning Outcomes

By researching and writing these three blogs, I strengthened my knowledge of:

- Data security and encryption in Amazon S3.
- Key management with AWS Key Management Service.
- Backup and recovery with AWS Backup.
- Item-level recovery for Amazon EBS and Amazon S3.
- Disaster recovery and data protection.
- Amazon EBS cost optimization.
- Performance monitoring with Amazon CloudWatch.
- Infrastructure as Code and operations automation.
- Practical skills for Developer, DevOps, and Cloud Engineer roles.
