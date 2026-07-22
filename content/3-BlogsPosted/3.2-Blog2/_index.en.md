---
title: "AWS Backup – Item-Level Recovery for Amazon EBS and Amazon S3"
date: 2026-07-21
weight: 2
chapter: false
pre: "<b>3.2. </b>"
---

# AWS Backup – Item-Level Recovery for Amazon EBS and Amazon S3

Hello everyone!

Accidental deletion is a common operations problem. The missing data may be only one configuration file, PDF document, CSV report, log file, or S3 object. However, when the only available recovery mechanism is a full snapshot or full backup, the operations team may have to restore a complete volume just to retrieve one item.

AWS Backup Search and Item-Level Recovery address this problem by allowing teams to search backup metadata and restore specific files or objects from AWS Backup-managed Amazon EBS and Amazon S3 recovery points.

## 1. Limitations of Traditional Recovery

A traditional EBS snapshot recovery process often requires an administrator to:

1. Identify the correct snapshot.
2. Restore the snapshot as a new EBS volume.
3. Attach the volume to an EC2 instance.
4. Mount the file system.
5. Find the required file.
6. Copy it back to the original system.
7. Detach and delete temporary resources.

This process can increase the Recovery Time Objective (RTO), create temporary storage and compute costs, and involve several manual steps.

For Amazon S3, locating the right object across many recovery points can also be time-consuming without searchable indexes and consistent metadata.

## 2. What Is Item-Level Recovery?

Item-Level Recovery is the ability to restore an individual file, directory, or object instead of restoring the entire backup.

AWS Backup supports metadata search for:

- Amazon EBS snapshots managed by AWS Backup.
- Amazon S3 backups managed by AWS Backup.

After locating an item, an administrator can start a restore job for the selected data.

Common use cases include:

- Recovering an accidentally deleted configuration file.
- Retrieving an older CSV or Excel report.
- Restoring documents for an audit.
- Locating application logs, videos, or files by path.
- Recovering a small group of objects after an operator error.
- Reducing the investigation and recovery time during an incident.

## 3. How Backup Indexing Works

A recovery point must have a **Backup Index** before its individual items can be searched.

The index catalogs file or object metadata. AWS Backup does not need to index the contents inside each file to provide metadata-based search.

Searchable properties can include:

### Amazon EBS

- File path.
- Creation time.
- Last modification time.
- File size.
- File-system or partition information.

### Amazon S3

- Object key.
- Object size.
- Creation time.
- ETag.
- Version ID.

Index creation can be enabled in a Backup Plan so that new backups are indexed automatically. Administrators can also create an index for an eligible existing recovery point.

## 4. Implementation Process

### Step 1: Prepare a Backup Vault and Backup Plan

Create or select a Backup Vault and define a Backup Plan for EBS or S3.

Important planning items include:

- Backup frequency.
- Retention period.
- Lifecycle and storage tier.
- Cross-Region or cross-account copies.
- Resource selection by tags.
- Recovery Point Objective (RPO).
- Compliance and immutability requirements.

### Step 2: Enable Backup Indexing

Enable Backup Index when creating an on-demand backup or configuring a backup rule.

For an eligible existing recovery point:

1. Open the AWS Backup console.
2. Choose **Backup vaults**.
3. Open the vault containing the recovery point.
4. Select the recovery point.
5. Choose **Create index**.

Before relying on this feature, verify current support for the resource type, Region, vault type, storage tier, file system, and encryption configuration.

### Step 3: Configure IAM

For EBS indexing, the AWS Backup role can require permissions such as:

```text
ec2:DescribeSnapshots
ebs:ListSnapshotBlocks
ebs:GetSnapshotBlock
kms:Decrypt
```

AWS provides the managed policy:

```text
AWSBackupServiceRolePolicyForIndexing
```

A search operator can use:

```text
AWSBackupSearchOperatorAccess
```

For EBS file-level recovery to Amazon S3, use a role with the required EBS, S3, and KMS permissions. AWS provides:

```text
AWSBackupServiceRolePolicyForItemRestores
```

Always verify KMS key policies in addition to the IAM role when the recovery point or destination bucket uses a customer managed key.

### Step 4: Create a Search Job

From **Search** in the AWS Backup console:

1. Select the resource type, such as EBS or S3.
2. Select the indexed recovery points.
3. Enter search criteria.
4. Start the search job.
5. Review and filter the results.

Examples:

- Find paths containing `config`.
- Find object keys ending in `.csv`.
- Find files created during a specific time range.
- Find objects larger than a defined size.

AWS Backup retains completed search results for a limited period, so export important results to an S3 bucket when they must be retained longer.

### Step 5: Restore the Selected Item

#### Restore Files from an EBS Snapshot

EBS file-level recovery restores the selected files or folders to an Amazon S3 bucket rather than overwriting the original EBS volume directly.

The AWS Backup console supports up to five paths in one file-level restore operation. The command line can be used for multiple paths according to the current service parameters and quotas.

A safe workflow is:

1. Restore the selected files to S3.
2. Verify integrity and ownership.
3. Scan the files for malware when appropriate.
4. Download or copy the files to the target system.
5. Record the recovery action for audit purposes.

EBS snapshots in archive or cold storage cannot be used directly for file-level recovery until they are in a supported state.

#### Restore Objects from an S3 Backup

An S3 backup can be restored to:

- The source bucket.
- An existing destination bucket.
- A newly created bucket.

AWS Backup supports item-level restore for a limited number of objects or folders per restore job through the console. The destination bucket must meet current AWS Backup requirements, including S3 Versioning where required.

Plan carefully when restoring to the source bucket so that object versions and current application data are preserved as intended.

## 5. Encrypting Restored Data

For EBS file-level recovery to Amazon S3, available choices can include:

- The destination bucket's default encryption.
- SSE-S3.
- SSE-KMS.

When SSE-KMS is selected, the restore role and KMS key policy must permit use of the key.

This is especially important in production environments that isolate data by application, environment, or tenant.

## 6. Benefits for DevOps and Operations

### Shorter RTO

Operators can search metadata and restore the required file without rebuilding a full EBS volume workflow.

### Fewer Temporary Resources

In many cases, the team no longer needs a complete temporary EBS volume and EC2 mount process to retrieve a few files.

### Better Incident Investigation

Search by path, time, size, object key, or version metadata can support audits, forensics, and incident response.

### Centralized Management

AWS Backup provides a central interface for backup plans, recovery points, indexes, search jobs, and restore jobs.

### Improved Automation

Enabling indexes through a Backup Plan prepares new recovery points for future search and recovery operations.

## 7. Cost and Limitation Considerations

AWS Backup can charge for:

- Creating backup indexes.
- Storing indexes.
- Running search jobs.
- Restoring items.
- Backup storage and destination storage.

Indexing every recovery point is not always cost-effective. Prioritize workloads with strict file-recovery requirements or high-value data.

Also consider:

- EBS file-level restore support varies by snapshot state and file system.
- Archived EBS snapshots are not directly eligible for file-level restore.
- Feature availability and quotas vary by Region.
- IAM and KMS permissions must be tested in advance.
- Backup search does not replace a complete disaster recovery design.
- Restore procedures must be tested regularly.

## 8. Practical Implementation Checklist

- [ ] Identify workloads that need item-level recovery.
- [ ] Define backup frequency and retention.
- [ ] Enable Backup Index for important recovery points.
- [ ] Apply consistent tags to EC2, EBS, S3, and Backup Vault resources.
- [ ] Configure IAM policies for indexing, searching, and restoring.
- [ ] Validate KMS key policies.
- [ ] Run a test search job.
- [ ] Run a test file-level restore to S3.
- [ ] Verify restored data.
- [ ] Monitor index and search cost.
- [ ] Create an incident recovery runbook.
- [ ] Test restore procedures on a schedule.

## 9. Combining It with a Ransomware-Resilience Strategy

Item-Level Recovery should be combined with:

- AWS Backup Vault Lock.
- Cross-account backups.
- Cross-Region copies.
- Least-privilege IAM.
- AWS CloudTrail.
- Malware scanning capabilities where applicable.
- Restore testing.
- Approval controls for deleting recovery points.

The goal is not only to create backups, but also to ensure that backups cannot be altered unexpectedly and can be restored within the required recovery window.

## Conclusion

AWS Backup Search and Item-Level Recovery improve the way DevOps teams handle small-scale data loss. Instead of restoring an entire EBS volume or manually inspecting many recovery points, administrators can search indexed metadata and restore selected files or S3 objects.

The feature can reduce RTO, minimize manual work, and reduce temporary-resource usage. Successful adoption still requires a well-designed Backup Plan, appropriate IAM and KMS permissions, a cost-aware indexing strategy, and regular restore testing.

## References

- [AWS Storage Blog – Streamline search and item-level recovery with AWS Backup](https://aws.amazon.com/blogs/storage/streamline-search-and-item-level-recovery-with-aws-backup/)
- [AWS Backup Developer Guide – Backup search](https://docs.aws.amazon.com/aws-backup/latest/devguide/backup-search.html)
- [AWS Backup Developer Guide – Restore an Amazon EBS volume](https://docs.aws.amazon.com/aws-backup/latest/devguide/restoring-ebs.html)
- [AWS Storage Blog – Enable item-level search and recovery for Amazon EC2](https://aws.amazon.com/blogs/storage/enable-item-level-search-and-recovery-for-amazon-ec2-with-aws-backup/)

#AWS #AWSBackup #AmazonEBS #AmazonS3 #DisasterRecovery #DataProtection #DevOps #CloudComputing
