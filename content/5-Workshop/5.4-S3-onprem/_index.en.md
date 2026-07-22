---
title: "Integrate the Frontend, Cognito, and API"
date: 2026-07-22
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---

# Integrate the Frontend, Cognito, and API

## Environment Variables

```env
VITE_API_URL=https://API_ID.execute-api.ap-southeast-1.amazonaws.com
VITE_AWS_REGION=ap-southeast-1
VITE_COGNITO_USER_POOL_ID=ap-southeast-1_xxxxx
VITE_COGNITO_CLIENT_ID=xxxxxxxx
```

Never place an AWS Secret Access Key in frontend code.

## JWT Header

```http
Authorization: Bearer ACCESS_TOKEN
Content-Type: application/json
```

## Build React

```powershell
cd frontend
npm install
npm run build
```

## Upload to S3

```powershell
aws s3 sync .\dist s3://BUCKET_NAME --delete
```

Then create a CloudFront distribution with S3 Origin Access Control, HTTPS, and SPA fallback to `index.html`.
