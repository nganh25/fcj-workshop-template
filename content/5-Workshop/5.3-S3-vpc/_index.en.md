---
title: "Run and Deploy the Backend"
date: 2026-07-22
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---

# Run and Deploy the Backend

## Build and Deploy

```powershell
cd C:\Users\nguye\smart-attendance-saas\backend
sam validate --lint
sam build
sam deploy --guided
```

Suggested values:

```text
Stack Name: smart-attendance-saas
Region: ap-southeast-1
Allow SAM CLI IAM role creation: Y
Save arguments to samconfig.toml: Y
```

## View Outputs

```powershell
aws cloudformation describe-stacks `
  --stack-name smart-attendance-saas `
  --query "Stacks[0].Outputs" `
  --output table
```

## Test the API

```powershell
Invoke-RestMethod -Method Get -Uri "API_URL/health"
```

Common errors:

- `401`: missing or invalid JWT.
- `403`: blocked by IAM, an authorizer, or WAF.
- `404`: incorrect route or stage.
- `500/502`: Lambda failure or invalid response format.
