---
title: "AWS Security & S3 – Amazon S3 Has Disabled SSE-C by Default Since April 2026"
date: 2026-07-20
weight: 1
chapter: false
pre: "<b>3.1. </b>"
---

# AWS Security & S3 – Amazon S3 Has Disabled SSE-C by Default Since April 2026

Hello everyone!

Amazon Simple Storage Service (Amazon S3) is one of the most widely used storage services on AWS. It supports many workload types, including static websites, document storage, backups, data lakes, application logs, and data used by AI/ML systems.

In addition to scalability and high data durability, security is a core part of S3 architecture. In April 2026, AWS deployed a new default setting: **Server-Side Encryption with Customer-Provided Keys (SSE-C) is blocked for new write requests on all newly created general purpose buckets**. AWS also applied the setting to existing buckets in accounts that had no SSE-C-encrypted objects.

This change does not remove SSE-C from Amazon S3. Workloads with a specific requirement can still enable it deliberately at the bucket level.

## 1. What Is SSE-C?

SSE-C is a server-side encryption method in which the customer supplies the encryption key with each request. Amazon S3 uses the key to encrypt or decrypt the object, but S3 does not retain the customer-provided key.

An application uploading an object with SSE-C typically sends these headers:

```text
x-amz-server-side-encryption-customer-algorithm
x-amz-server-side-encryption-customer-key
x-amz-server-side-encryption-customer-key-MD5
```

SSE-C gives the organization full control over the key. That control also means the application is responsible for secure storage, distribution, rotation, and availability of the key. If the key is lost or the wrong key is supplied, the object cannot be decrypted.

## 2. Important Changes Introduced in April 2026

### 2.1. New Buckets Block SSE-C by Default

Every newly created general purpose bucket blocks new object writes that use SSE-C by default. An application that must continue using SSE-C has to change the bucket configuration explicitly.

### 2.2. Some Existing Buckets Were Updated

For AWS accounts with no existing SSE-C-encrypted objects, AWS also blocked SSE-C for new write requests on existing general purpose buckets.

If an account already contained at least one SSE-C-encrypted object, AWS did not automatically change the account's existing buckets. New buckets in that account are still created with SSE-C blocked by default.

### 2.3. Write Requests Can Receive HTTP 403

When SSE-C is blocked, operations such as `PutObject`, `CopyObject`, `POST Object`, multipart uploads, or replication requests that use SSE-C are rejected with:

```text
HTTP 403 AccessDenied
```

This can interrupt upload pipelines, backup scripts, file-sharing applications, or older integrations that still attach SSE-C headers.

### 2.4. Existing SSE-C Objects Are Not Re-encrypted

The setting applies to new write requests. Amazon S3 does not change the encryption method of existing SSE-C objects.

Applications can still call `GetObject` or `HeadObject` for existing objects when they provide the correct SSE-C headers and the original key.

## 3. Why Did AWS Make This Change?

SSE-C offers strong customer control but introduces operational risk:

- The application must securely store and distribute encryption keys.
- The key must be provided for every relevant read or write request.
- Losing the key means losing access to the encrypted data.
- Key rotation, auditing, and authorization are more difficult to centralize.
- A configuration error can interrupt an entire data pipeline.

For most modern workloads, AWS recommends keeping SSE-C blocked unless a specific technical or compliance requirement exists. SSE-S3 and SSE-KMS are generally easier to operate and integrate more naturally with AWS security services.

## 4. What Should Developers and DevOps Teams Do?

### Step 1: Inspect Bucket Configuration

Use AWS CLI to review the bucket encryption configuration:

```bash
aws s3api get-bucket-encryption \
  --bucket your-bucket-name
```

The response can include `BlockedEncryptionTypes`. When its value contains `SSE-C`, new SSE-C write requests are blocked.

The IAM principal performing the check requires:

```text
s3:GetEncryptionConfiguration
```

### Step 2: Review Source Code and Pipelines

Search application code, environment variables, CI/CD configuration, and operations scripts for:

```text
SSECustomerAlgorithm
SSECustomerKey
SSECustomerKeyMD5
x-amz-server-side-encryption-customer-
```

Review all relevant components, including:

- Older AWS SDK integrations.
- AWS CLI scripts.
- Data synchronization tools.
- Backup agents.
- File-upload pipelines.
- Data ingestion systems.
- Third-party applications connected to S3.

### Step 3: Re-enable SSE-C Only When Required

When a workload has a confirmed requirement for SSE-C, an administrator can allow it again:

```bash
aws s3api put-bucket-encryption \
  --bucket your-bucket-name \
  --server-side-encryption-configuration '{
    "Rules": [{
      "BlockedEncryptionTypes": {
        "EncryptionType": "NONE"
      }
    }]
  }'
```

The IAM principal requires:

```text
s3:PutEncryptionConfiguration
```

To block SSE-C again:

```bash
aws s3api put-bucket-encryption \
  --bucket your-bucket-name \
  --server-side-encryption-configuration '{
    "Rules": [{
      "BlockedEncryptionTypes": {
        "EncryptionType": "SSE-C"
      }
    }]
  }'
```

Test the change in a non-production environment before applying it to critical data flows.

### Step 4: Update Infrastructure as Code

Do not change a bucket manually while leaving CloudFormation, AWS CDK, or Terraform with a conflicting configuration. The desired state must be represented in Infrastructure as Code.

Example AWS CloudFormation:

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

Example AWS CDK with TypeScript:

```typescript
const bucket = new s3.Bucket(this, "ApplicationBucket", {
  blockedEncryptionTypes: [
    s3.BlockedEncryptionType.SSE_C,
  ],
});
```

When the workload must allow SSE-C, use the appropriate `NONE` setting supported by the IaC tool and validate the generated CloudFormation template.

## 5. Should You Use SSE-S3 or SSE-KMS Instead?

### SSE-S3

SSE-S3 is suitable when the workload needs a simple encryption solution without managing a separate KMS key. Amazon S3 automatically encrypts new objects with SSE-S3 when no other method is specified.

Advantages:

- Simple operation.
- No customer-managed KMS key is required.
- No additional KMS request cost.
- Suitable for static content, standard application data, logs, and many backup workloads.

### SSE-KMS

SSE-KMS is suitable for production environments that need stronger control and auditing.

Advantages:

- Authorization through IAM and KMS key policies.
- Key-usage visibility through AWS CloudTrail.
- Support for customer managed keys.
- Automatic key rotation for eligible customer managed keys.
- Better alignment with workloads that have strict security or compliance requirements.

For high request volumes, consider **S3 Bucket Keys** to reduce direct AWS KMS request traffic and lower KMS-related cost.

## 6. Safe Implementation Checklist

- [ ] Inventory S3 buckets in every account and Region.
- [ ] Inspect `BlockedEncryptionTypes`.
- [ ] Search source code and pipelines for SSE-C headers.
- [ ] Review CloudTrail activity for SSE-C requests.
- [ ] Confirm whether the workload has a real SSE-C requirement.
- [ ] Migrate to SSE-S3 or SSE-KMS when SSE-C is not required.
- [ ] Update CloudFormation, CDK, or Terraform.
- [ ] Test upload, download, multipart upload, copy, and replication flows.
- [ ] Create monitoring for repeated `403 AccessDenied` errors.
- [ ] Prepare a rollback plan before changing production workloads.

## Conclusion

Amazon S3's April 2026 default is a security improvement designed to reduce accidental use and misconfiguration of customer-provided keys. SSE-C remains available, but it must be enabled deliberately when a workload has a specific requirement.

For Developers and DevOps teams, the key actions are to identify existing SSE-C usage, update Infrastructure as Code, and select the encryption method that fits the workload. In most cases, SSE-S3 or SSE-KMS provides simpler operations, better auditability, and a lower risk of permanent data loss caused by a missing key.

## References

- [Amazon S3 – Default SSE-C setting FAQ](https://docs.aws.amazon.com/AmazonS3/latest/userguide/default-s3-c-encryption-setting-faq.html)
- [Amazon S3 – Blocking or unblocking SSE-C](https://docs.aws.amazon.com/AmazonS3/latest/userguide/blocking-unblocking-s3-c-encryption-gpb.html)
- [AWS What's New – S3 default bucket security setting](https://aws.amazon.com/about-aws/whats-new/2026/04/s3-default-bucket-security-setting/)
- [AWS CloudFormation – BlockedEncryptionTypes](https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-properties-s3-bucket-blockedencryptiontypes.html)

#AWS #AmazonS3 #CloudSecurity #SSEKMS #SSEC #DevOps #CloudComputing #AWSStorage
