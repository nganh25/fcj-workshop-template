---
title: "Testing, Security, and Monitoring"
date: 2026-07-22
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---

# Testing, Security, and Monitoring

## Testing

| Scenario | Expected result |
|---|---|
| Missing JWT | 401/403 |
| First clock-in | Record created |
| Duplicate clock-in | 409 or business error |
| Clock-out before clock-in | Rejected |
| Attendance history | Only the correct user/tenant data |

## Security

- Resolve `userSub` and tenant information from trusted claims or mappings.
- Do not commit `.env` files or credentials.
- Apply IAM least privilege.
- Use Secrets Manager and KMS when required.
- Keep the frontend S3 bucket private behind CloudFront OAC.

## Monitoring

Monitor API Gateway `4XX/5XX/Latency`, Lambda `Errors/Duration/Throttles`, DynamoDB `ThrottledRequests/Latency`, and SQS dead-letter queues.

```powershell
sam logs --stack-name smart-attendance-saas --name FUNCTION_NAME --tail
```
