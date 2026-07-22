---
title: "Triển khai và kiểm thử API"
date: 2026-07-22
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---

# Triển khai và kiểm thử API

## 1. Deploy

```powershell
sam deploy --guided
```

Giá trị gợi ý:

```text
Stack Name: smart-attendance-workshop
AWS Region: ap-southeast-1
Confirm changes before deploy: Y
Allow SAM CLI IAM role creation: Y
Disable rollback: N
Save arguments: Y
```

## 2. Lấy Output

```powershell
$STACK = "smart-attendance-workshop"

$API_URL = aws cloudformation describe-stacks `
  --stack-name $STACK `
  --query "Stacks[0].Outputs[?OutputKey=='ApiUrl'].OutputValue" `
  --output text

$USER_POOL_ID = aws cloudformation describe-stacks `
  --stack-name $STACK `
  --query "Stacks[0].Outputs[?OutputKey=='UserPoolId'].OutputValue" `
  --output text

$CLIENT_ID = aws cloudformation describe-stacks `
  --stack-name $STACK `
  --query "Stacks[0].Outputs[?OutputKey=='UserPoolClientId'].OutputValue" `
  --output text
```

## 3. Kiểm tra health

```powershell
Invoke-RestMethod -Method Get -Uri "$API_URL/health"
```

## 4. Tạo người dùng thử

```powershell
$EMAIL = "employee@example.com"
$PASSWORD = "Workshop@2026"

aws cognito-idp admin-create-user `
  --user-pool-id $USER_POOL_ID `
  --username $EMAIL `
  --user-attributes Name=email,Value=$EMAIL Name=email_verified,Value=true `
  --message-action SUPPRESS

aws cognito-idp admin-set-user-password `
  --user-pool-id $USER_POOL_ID `
  --username $EMAIL `
  --password $PASSWORD `
  --permanent
```

## 5. Lấy Token

```powershell
$AUTH_JSON = aws cognito-idp initiate-auth `
  --client-id $CLIENT_ID `
  --auth-flow USER_PASSWORD_AUTH `
  --auth-parameters "USERNAME=$EMAIL,PASSWORD=$PASSWORD"

$TOKEN = ($AUTH_JSON | ConvertFrom-Json).AuthenticationResult.AccessToken
$HEADERS = @{ Authorization = "Bearer $TOKEN" }
```

## 6. Gọi API

```powershell
Invoke-RestMethod -Method Post -Uri "$API_URL/attendance/checkin" -Headers $HEADERS
Invoke-RestMethod -Method Post -Uri "$API_URL/attendance/checkout" -Headers $HEADERS
Invoke-RestMethod -Method Get  -Uri "$API_URL/attendance/history" -Headers $HEADERS
```

Kết quả cần kiểm tra:

- Không có token: `401`.
- Check-in lần đầu: `201`.
- Check-in lần hai trong ngày: `409`.
- Check-out thành công: `200`.
- Lịch sử chỉ chứa dữ liệu của người đang đăng nhập.
