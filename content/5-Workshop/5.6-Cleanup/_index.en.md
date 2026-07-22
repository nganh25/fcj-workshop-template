---
title: "Resource Cleanup"
date: 2026-07-22
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---

# Resource Cleanup

## CloudFront and S3

1. Disable and delete the CloudFront distribution.
2. Delete objects from the frontend and report buckets.
3. Remove object versions if S3 Versioning is enabled.

```powershell
aws s3 rm s3://BUCKET_NAME --recursive
```

## Delete the SAM Stack

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend
sam delete --stack-name smart-attendance-saas --region ap-southeast-1
```

## Check for Remaining Resources

Review CloudFormation, Lambda, API Gateway, Cognito, DynamoDB, SQS, Step Functions, EventBridge, SES, S3, CloudFront, CloudWatch, Secrets Manager, and KMS.
