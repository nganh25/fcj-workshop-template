---
title: "Event 2 – Smart Attendance SaaS Final Demo & Technical Review"
date: 2026-07-10
weight: 2
chapter: false
pre: "<b>4.2. </b>"
---

# Smart Attendance SaaS Final Demo & Technical Review

## Event Information

- **Date:** July 10, 2026
- **Format:** Final project presentation and technical review
- **Role:** Team member responsible for cloud infrastructure and cloud networking content

## Presentation Topics

- Problem statement and MVP scope.
- Route 53, CloudFront, S3, Cognito, API Gateway, Lambda, and DynamoDB architecture.
- Clock-in, clock-out, attendance, profile, and report flows.
- Asynchronous processing with Step Functions, SQS, and a DLQ.
- Email delivery through EventBridge, a Lambda worker, and SES.
- Security with WAF, Shield, IAM, KMS, and Secrets Manager.
- CI/CD with CloudFormation, CodeBuild, and CodePipeline.
- Monitoring with CloudWatch and X-Ray.

## My Contributions

- Explained connectivity across the edge, authentication, API, compute, and data layers.
- Reviewed IAM roles, security configuration, and tenant isolation.
- Supported deployment, API testing, and environment troubleshooting.
- Prepared the architecture diagram and technical presentation content.

## Lessons Learned

- Architecture must balance completeness with the MVP scope.
- A demo requires a clear script and stable test data.
- Monitoring and logging are core system components, not optional additions.
- Architecture documentation aligns team understanding and reduces integration errors.
